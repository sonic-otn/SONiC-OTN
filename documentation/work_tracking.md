
# Project Working Document

This document aims to capture information on all project activities, especially the design and the other decisions by sonic-otn-wg. It should be up-to-date always. 

Each items below should have a status of:
* TBD.
* in progress.
* completed.

## HLD (in progress)

## OTAI (in progress)
* LAI -> OTAI name change.
* Make generic hardware management as customized APIs. These functionalities are covered in existing pmon container.

## sonic-platform-daemon sonic-platform-common (in progress)
* Restore deleted original code of platform python base classes and pmon daemons (PSU, Fan, Thermal, xcvr).
* Change newly created platform python classes as existing platform subclasses to reduce duplicated code.
* Newly created daemon can be optionally launched at run time, based on device config (pmon_daemon_control.json).

## sonic-management-framework sonic-management-common (TBD)
## sonic-swss-common sonic-olss sonic-lairedis
## sonic-utility sonic-telemetry
## sonic-buildimage ONIE