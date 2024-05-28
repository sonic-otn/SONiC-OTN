# Optical Transport Syncd (Syncd-OT) High Level Design #

## Table of Content 

### Revision  
Rev | Rev	Date	| Author	| Change Description
---------|--------------|-----------|-------------------
|v0.1 |28/03/2024  |Weitang Zheng | Initial version

### Scope  

This document describes the high level design of Syncd-OT in SONiC

### Definitions/Abbreviations 

Definitions/Abbreviation|Description
------------------------|-----------
OTSS| Optical Transport State Service
OTAI| Optcial Transport Abstraction Interface
OTN| Optical Transport Network
CRUD| CREATE, READ, UPDATE and DELETE
OA| Optical Amplifier
OSC| Optical Supervisory Channel
OLP| Optical Line Protection
VOA|Optical Attenuator
OTDR| Optical Time Domain Reflectometer



### Overview 

Syncd-OT provides a mechanism to allow the synchronization of the optical transport network state with actual optical transport components and hardware. It includes the initialization, the configuration, the PM collections and the alarms of the optical components.

### 1 Requirements

The Syncd-OT provides these functionalities:
 * Optical components initialization and configuration.
 * Synchronize optical components hardware status to redis database. 
 * Performance monitoring (PM), with 15 minutes and 1 day statistics and historical counters .
 * Supports alarms notification and OCM, OTDR, OLP data reporting notifications
 * Supports linecard, syncd-ot container restart. 

### 2 Architecture Design 

Syncd-OT interacts with OTSS to manage OTN linecard and components, and synchronizes real hardware status, PMs, alarms to the redis database. There are 4 modules in the syncd-OT application.
* CRUD handler  
  CRUD handler processes all the CRUD request message from OTSS, calls the vendor's OTAI library to manage the optical components.

* Flexcounter and PM  
  Flexcounter keeps polling the optical components status and statistics with the settings which are configured by OTSS. The PM module accumulates the Flexcounter result with 15 minutes and 1 day interval. Historical PM data is saved in History database as well.

* Notification handler  
  Notification handler defines the callbacks for OTAI notifications. For instance, the alarm notifications, the OCM spectrum data reporting notification, the OTDR scanning data reporting notification, etc.  

* linecard and Syncd-OT re-initialization.  
  After optical linecard or Syncd-OT rebooting, Syncd-OT needs to re-initialize the optical linecard's configuration.
 
<img src="../assets/arch-syncd-ot.png" alt="syncd-ot architecture" style="zoom: 35%;" />

#### 2.1 CRUD handler 

OTSS sends optical linecard and components CRUD request to Syncd-OT. Syncd-OT handles these requests by calling the vendor OTAI library, and saves these OTAI objects and attributes in the ASIC_DB.  
<img src="../assets/Syncd-OT_CRUD_Handler.png" alt="syncd-ot CRUD handler" style="zoom: 30%;" />

1. Syncd-OT application subscribes the redis communication channel with OTSS during the initialization process.
2. Syncd-OT enter into the main loop process, which keep receiving and handling the CRUD requests from OTSS.
3. OTSS sends the CRUD requests to redis communication channel when it calls these four APIs, `createOtaiObject`,  `setOtaiObjectAttrs`, `getOtaiObjectAttrs` and `getOtaiObjectAttrs`
4. Once Syncd-OT received the CRUD request from the redis communication channel, the `ProcessEvent` function starts to handle these request.
5. Based on the CRUD request type, these request are dispatched to different handler functions, `processOidCreate`,`processOidSet`,`processOidGet` and `processOidRemove`.
6. these handler functions call vendor's OTAI library APIs to create, set, get and remove optical components attributes and objects.
7. These message handler send the CRUD response to OTSS.
8. For create, set and remove message handler, it will create, set and remove ASIC objects and attributes, then save these modifications to ASIC_DB.

#### 2.2 FlexCounter and PM
FlexCounter in Syncd-OT keeps polling the optical components' status and PM statistics, the PM module accumulates these sampled data within 15 minutes and 24 hours interval. The sampled status are saved to state_db, the accumulated PM data are saved to counter_db and history_db.

<img src="../assets/Syncd-OT_FlexCounter_PM.png" alt="Syncd-OT FlexCounter PM" style="zoom: 50%;" />

The `flexcounter.json` contains the FlexCounter configurations per optical linecard. It defines the three kinds of FlexCounter groups, Vendor and users can change the FlexCounter group's configuration, for example, the sampling interval.
1. STAT_STATUS, the counter for optical components status sampling.
2. STAT_GAUGE, the counter for optical components analog data sampling.
3. STAT_COUNTER, the counter for optical components counter data sampling.

The `flexcounter.json` contains the counter IDs for each target optical objects. Here is an example of linecard gauge configuration, it includes four gauge counter IDs, memory available, memory utilized, cpu utilization and temperature. The Flexcounter module in OTSS can sample these user assigned counter IDs. User can customize these list to enable different sampling groups and OTAI attributes. Vendor can customize all amplifiers's status sampling attributes in the `OA_COUNTER_ID_LIST_STATUS` group, and customize interface package counters attributes for an OSC interface in `INTERFACE_COUNTER_ID_LIST_COUNTER`.

```json
"LINECARD_COUNTER_ID_LIST_GAUGE": {
"attr1": "OTAI_LINECARD_STAT_MEMORY_AVAILABLE",
"attr2": "OTAI_LINECARD_STAT_MEMORY_UTILIZED",
"attr3": "OTAI_LINECARD_STAT_CPU_UTILIZATION",
"attr4": "OTAI_LINECARD_STAT_TEMPERATURE"
},
"OA_COUNTER_ID_LIST_STATUS": {
"attr1": "OTAI_OA_ATTR_OPER_STATUS",
"attr2": "OTAI_OA_ATTR_TARGET_GAIN",
"attr3": "OTAI_OA_ATTR_TARGET_GAIN_TILT",
"attr4": "OTAI_OA_ATTR_GAIN_RANGE"
},
"INTERFACE_COUNTER_ID_LIST_COUNTER": {
"attr1": "OTAI_INTERFACE_STAT_OUT_PKTS",
"attr2": "OTAI_INTERFACE_STAT_IN_PKTS"
},
```

The `FlexCounterOrch` module in OTSS loads the `flexcounter.json` file, and save these FlexCounter Groups and Counter IDs in the FlexCounter database.  

The FlexCounter module in Syncd-OT subscribes the FlexCounter database, and initializes the FlexCounter module based on these configurations. It starts threads to sample all these Counter IDs by calling the OTAI library, the sampled status data is saved in state_db.

The PM module accumulates these sampled gauge and counter data within 15 minutes and 24 hours interval, and saves these current PM data to counter_db. When the timer reached 15 minutes and 24 hours window, these PM data are saved to the history_db as well.

#### 2.3 Notification handler
The notification handler provides a mechanism to register and handle the events from an optical linecard. For instance, the alarm notification, the OCM spectrum data reporting notification, the OTDR scanning data reporting notification, etc.  

OTAI defines multiple notification API functions:
* OTAI_LINECARD_ATTR_LINECARD_ALARM_NOTIFY
* OTAI_LINECARD_ATTR_LINECARD_OCM_SPECTRUM_POWER_NOTIFY
* OTAI_LINECARD_ATTR_LINECARD_OTDR_RESULT_NOTIFY
* OTAI_LINECARD_ATTR_LINECARD_STATE_CHANGE_NOTIFY
* OTAI_LINECARD_NOTIFICATION_NAME_LINECARD_STATE_CHANGE

When Syncd-OT handles the linecard creation request message in CRUD module, it assigns these notification API's callback functions to vendor's OTAI library. Once the target events are triggered in optical linecard, vendor's OTAI library can call these notification callback functions to handle these events. For example, bellow flow illustrates the alarm notification handling process.

<img src="../assets/Syncd-OT_Alarm_Notification.png" alt="Syncd-OT Alarm Notification.png" style="zoom: 50%;" />

1. Syncd-OT registers all linecard notification callbacks during linecard creation.
2. When the linecard or optical components generates an alarm, vendor's OTAI library can catch the alarm notification
3. Vendor's OTAI library call the registered callback function `onLinecardAlarm`
4. In the callback function `onLinecardAlarm`, it saves the generated alarm to state_db with the table name `CURALARM`
5. When the linecard or optical components clears an alarm, vendor's OTAI library can catch the alarm cleared notification
6. Vendor's OTAI library call the registered callback function `onLinecardAlarm`
7. In the callback function `onLinecardAlarm` delete the alarm in state_db, then save the cleared alarm in the history_db with table name `HISALARM`

#### linecard and Syncd-OT re-initialization
In the OTN system, user can restart the optical linecard and the Syncd-OT docker container, then the OTN system needs to re-initialize the Syncd-OT container and the optical linecard. For instance, recreate and configure all the optical components with the attributes in the ASIC_db, re-initialize the Flexcounter based on the configurations in FlexCounter database. 

If the SYncd-OT container restart, the Syncd-OT calls the `HardReiniter` function to re-initialize all the OTAI objects and Flexcounter module. 

If the linecard reboot, the OTAI library can detect the heartbeat and trigger the `OTAI_LINECARD_NOTIFICATION_NAME_LINECARD_STATE_CHANGE` notification. When the linecard boot-up and status ready, OTAI library triggers the notification again. Once the status is ready, then Syncd-OT call the `SoftReniniter` functions to re-initialize all the OTAI objects and Flexcounter module. 

<img src="../assets/Syncd-OT_ReIniter.png" alt="Syncd-OT re-initialization.png" style="zoom: 50%;" />

1. After Syncd-OT reboot, it waits for the OTAI library status is operational ready.  
2. Syncd-OT calls the `HardReiniter` function to re-initialize OTAI objects and Flexcounter. It reads the ASIC state  from the ASIC_DB.  
3. It re-initialize the linecard first by calling the `processLinecards` function.  
4. In the `processLinecards` function, it recreates the linecard object based on the attributes in AISC_db.  
5. Once the linecard object is created, it calls vendor's OTAI library to set the other attributes.  
6. Then Syncd-OT starts to re-initialize the other OTAi objects in `processOids`  
7. In the `processOids` function, it loops all OTAI objects and calls vendor's OTAI library to recreate these objects.  
8. After all the other OTAI objects are created, it calls vendor's OTAI library to set the other attributes.  
9. Syncd-OT starts to reinitialize Flexcounter, it the Flexcounter groups and counter_ids from Flexcounter database  
10. Based on these configurations in Flecounter database, it calls the FlexCounterManager to add plugins and counters.  

