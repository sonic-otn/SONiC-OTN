
# SONiC-OTN Project Overview/Working in Progress

## Charter
sonic-otn-wg will focus on enabling SONiC to support optical transport equipment, including
* Optical transponders/muxponders.
* Optical line equipment such as optical amplifiers, wavelength selective switches.  
* Optical DWDM/Grey pluggable modules on switching/routing equipment.
* Packet-optical device (ex. [Cassini](https://telecominfraproject.com/oopt/)) with both packet and optical functionality (Stretched goal).

The following diagrams illustrate end to end network deployment with SONiC as open source NOS for both packet and optical network.

<img src="assets/sonic-otn-transponder-open-line-system.png" alt="transponder and open line system with sonic-otn" style="zoom: 50%;" />

<img src="assets/sonic-otn-packet-optical-system.png" alt="packet optical and open line system with sonic-otn" style="zoom: 40%;" />

## Estimated Duration
Two years, by December, 2024
```mermaid
---
displayMode: compact
---
gantt
title SONiC-OTN group timeline

SONiC-OTN Group Kick off    :done,  des1, 2023-01-01,90d
Source Code Open Sourced           :done,  des2, after des1, 180d
Submit SONiC-OTN to SONiC master      :crit,  des3, after des2, 240d
Support More SONiC-OTN platforms    :active,  des4, after des3, 180d
Target SONiC 202411 branch  :milestone, 2024-011-30, 0d

```

## Contribution Artifacts to SONiC Community
* Optical Transport Abstraction Interface (OTAI) Specification and header files. 
* Contribution to SONiC code base for support optical transport devices:
  - sonic-ot-vendorA.bin, for all devices of vendorA.
  - sonic-ot-vendorB.bin, for all devices of vendorB.
  - sonic-ot-vs.img.gz, Virtual optical device. 
  - ONIE images for optical device families.

With above deliverables, SONiC effectively becomes a network operating system (NOS) supporting both packet switch and optical transport white box. Work-in-progress source code repository in [Github](https://github.com/zhengweitang-zwt/sonic-buildimage).

## Documentations
All documents about this project can be find [here](documentation/README.md).

### OTAI Specification
This section provides a proposed specification for optical transport abstraction interface. It will be submitted to the community for review after a consensus is reached within sonic-otn working group.
Please find OTAI spec [here](documentation/OTAI-v0.0.1.md).

### High Level Design
Please find sonic-otn high level design [here](documentation/SONiC-OTN%20HLD%20and%20plan%20summary.pdf) which defines the design objectives and approaches of this project.

### Project management
You can find the sonic-otn project development status and tasks [here](https://github.com/users/sonic-otn/projects/1/views/2), and you can join the sonic-otn bi-weekly community meeting from the Calendar [here](https://sonic-net.github.io/SONiC/Calendar.html)
