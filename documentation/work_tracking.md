
# Project Working Document

This document aims to capture information on all project activities, especially the design and the other decisions by sonic-otn-wg. It should be up-to-date always. 

Each items below should have a status of:
* TBD.
* in progress.
* completed.

## HLD (in progress)
6/27/23: Discussion on [Molex proposal](./SONiC-OTN-HLD.md)

7/11/23: Discussion on [Infinera proposal]()

## OTAI API Spec (in progress)
* LAI -> OTAI name change. Adopted. [OTAI repo](https://github.com/zhengweitang-zwt/OTAI) created.
* 7/11/23: Generic hardware management should not be in OTAI. These functionalities are covered in existing pmon container. [Molex's feedback](./discussion/OTAI_feedback_molex.md).

## sonic-platform-daemon sonic-platform-common (in progress)
* Restore deleted original code of platform python base classes and pmon daemons (PSU, Fan, Thermal, xcvr).
* Change newly created platform python classes as existing platform subclasses to reduce duplicated code.
* Newly created daemon can be optionally launched at run time, based on device config (pmon_daemon_control.json).

## sonic-management-framework sonic-management-common (TBD)
## sonic-swss-common sonic-olss sonic-lairedis
## sonic-utility sonic-telemetry
## sonic-buildimage ONIE