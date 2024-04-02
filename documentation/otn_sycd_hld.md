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
 * Synchronize optical components hardware status to database. 
 * Performance monitoring (PM), with 15 minutes and 1 day statistics and historical counters .
 * Supports alarms notification and OCM, OTDR, OLP data reporting notification
 * Supports linecard, syncd-ot container restart. 

### 2 Architecture Design 

Syncd-OT interacts with OTSS to manage OTN linecard and components, and synchronizes real hardware status, PMs, alarms to the database. There are 4 modules in the syncd-OT application.
* CRUD handler  
  CRUD handler process all the CRUD request message from OTSS, call the vendor OTAI library to manage the optical components.

* Flexcounter and PM  
  Flexcounter polling the optical components status and statistics with the configurations which were initialed by OTSS. The PM module accumulate the flexcounter result with 15 minutes and 1 day interval. 

* Notification handler  
  Notification handler defines the callbacks for OTAI notification functions, for instance, the alarm notifications, the OCM spectrum data reporting notification, the OTDR scanning data reporting notification, etc.  

* OTAI libray  
  OTAI libray is the vendor implementation of OTAI.
 
<img src="../assets/arch-syncd-ot.png" alt="syncd-ot architecture" style="zoom: 30%;" />

#### 2.1 CRUD handler 

OTSS sends optical linecard and components CRUD request to Syncd-OT. Syncd-OT needs to handle these request by calling the vendor OTAI library, and save these OTAI objects and attributes in the ASIC_DB.  
	




