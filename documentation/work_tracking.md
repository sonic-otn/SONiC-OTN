
# Project Working Document

This document aims to capture information on all project activities, especially the design and the other decisions by sonic-otn-wg. It should be up-to-date always. 

Each items below should have a status of:
* TBD.
* in progress.
* completed.

## HLD (completed)
6/27/23: Discussion on [Molex proposal](../documentation/discussion/SONiC-OTN-HLD-discussion.md)

7/11/23: Discussion on [Infinera proposal]()

9/7/23: [Option 1](../documentation/SONiC-OTN%20HLD%20and%20plan%20summary.pdf) is chosen

## OTAI API Spec (Completed)
* LAI -> OTAI name change. Adopted. [OTAI repo](https://github.com/zhengweitang-zwt/OTAI) created.
* 7/11/23: Generic hardware management should not be in OTAI. These functionalities are covered in existing pmon container. [Molex's feedback](./discussion/OTAI_feedback_molex.md).
* 9/7: OTAI [initial version](../documentation/OTAI-0.0.1-spec.xlsx) will be pure openconfig yang model based.

## sonic-platform-daemon sonic-platform-common (in progress)
* Restore deleted original code of platform python base classes and pmon daemons (PSU, Fan, Thermal, xcvr).
* Change newly created platform python classes as existing platform subclasses to reduce duplicated code.
* Newly created daemon can be optionally launched at run time, based on device config (pmon_daemon_control.json).

## sonic-management-framework sonic-management-common (TBD)
## sonic-swss-common sonic-olss sonic-lairedis
## sonic-utility sonic-telemetry
## sonic-buildimage ONIE