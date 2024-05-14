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
 * Supports alarms notification and OCM, OTDR, OLP data reporting notification
 * Supports linecard, syncd-ot container restart. 

### 2 Architecture Design 

Syncd-OT interacts with OTSS to manage OTN linecard and components, and synchronizes real hardware status, PMs, alarms to the redis database. There are 4 modules in the syncd-OT application.
* CRUD handler  
  CRUD handler processes all the CRUD request message from OTSS, call the vendor's OTAI library to manage the optical components.

* Flexcounter and PM  
  Flexcounter keeps polling the optical components status and statistics with the settings which were configured by OTSS. The PM module accumulates the Flexcounter result with 15 minutes and 1 day interval. Historical PM data is saved in History database as well.

* Notification handler  
  Notification handler defines the callbacks for OTAI notification functions. For instance, the alarm notifications, the OCM spectrum data reporting notification, the OTDR scanning data reporting notification, etc.  

* OTAI libray  
  OTAI libray is the vendor implementation of OTAI.
 
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
8. For create, set and remove message handler, it will create, set and remove ASIC objects and save these modifications to ASIC_DB.


