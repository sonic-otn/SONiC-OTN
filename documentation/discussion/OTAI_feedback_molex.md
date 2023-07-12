# OTAI API Feedbacks

SAI abstracts packet switching functions of switch ASIC and it does not include any hardware implementation of the white box switch. OTAI should capture the common optical transport features implemented by optical components, such as EDFA, WSS, OCM, transponder, etc. Hardware implementation should not be in OTAI.

## Issue 1: Manufacturing Info and Generic Operations

In SONiC all generic hardware and operation are managed by pmon container not syncd (SAI) container. The following diagram illustrates the functional demarcation between syncd and pmon container. 

![alt text](https://github.com/jjin62/SONiC-OTN/blob/jjin/assets/pmon-vs-syncd.png?raw=true)

Generic hardware info in OTAI:
- MFG_NAME; MFG_DATE; SERIAL_NO; PART_NO; HARDWARE_VERSION; SOFTWARE_VERSION; FIRMWARE_VERSION; CLEI_CODE; VENDOR_PART; VENDOR_REV. 

These info are device vendor specific and prune to change. For keeping OTAI less churn, they are not suitable to be included in OTAI attributes.

Generic operations and status:
- ADMIN_STATE; OPER_STATUS; REMOVABLE; EMPTY.
- RESET; SW_UPGRADE.

SONiC choose allowing the device(white box) vendors to implement these functionalities as python subclass in pmon container. Note that SAI driver is ASIC platform based, while the drivers for above hardware management features are whitebox(device level) specific. [pmon API](https://github.com/sonic-net/SONiC/blob/master/doc/platform_api/new_platform_api.md) also defines generic hardware hierarchy from chassis, module (LC, SC, FC) and components

The management of generic hardware should be removed from OTAI and move to pmon container. As a result, OTAI should only contain the APIs that related to the optical services. Optical transport core functionalities are well defined OpenConfig optical transport [yang models](https://github.com/openconfig/public/tree/master/release/models/optical-transport). These yang models are now quasi-standard for optical features.

## Issue 2: Line Card as top level Object

Line card is a hardware implementation mechanism of network device. An optical device can be a fixed pizza box with no plug-in line card, or it can be a logical system that consists of many physical chassis. Therefore, line card is hardware implementation specific and should not be modeled as OTAI object. In SAI, `switch` is an abstract object which contains attributes of a switch device. It is top level object which must be initialized during SONiC start up. Equivalently, in OTAI, we should use a more functional and abstract name, instead of `linecard` as top level object.

### Option 1: Inherit `switch` object from SAI

  We can reuse `'switch` object in SONiC. As a result, `switch` now also cover optical devices. This approach is aligned with [SONiC-DASH](https://github.com/sonic-net/DASH/blob/main/documentation/general/dash-high-level-design.md) project which extend SONiC to support serveral DASH devices. SONiC device meta data can be used to define type of the device. For example, DASH appliance is defined as [following](https://github.com/sonic-net/DASH/blob/main/documentation/general/dash-sonic-hld.md):

 "DEVICE_METADATA": { 

    "localhost": {
        "subtype": "Appliance",
        "type": "SonicHost",
        "switch_type": "dpu",
        "sub_role": "None"
     }
}

sonic-otn project can take a similar approach. This is a preferred approach which would help us seamlessly extend current SONiC code base. 

### Option 2: Using `module` as top level object.  
  This approach follows [TAI](https://github.com/Telecominfraproject/oopt-tai) specification, where the transponder device is called as `module`. 
  - If OTN vendor chooses multi-ASIC architecture, `module` can be referred to a linecard. Actually in pmon `module` means line card, supervisor card or fabric card.
  - In a single ASIC architecture, `module` could refer to the optical device itself. For example optical amplifier is often called as `AMP module`.

In either options, an oati object is identified with `switch` or `module` id, object type and object index. Optical vendors should be able to choose single or multi-ASIC architecture to build sonic-otn image.
  
## Action Items for the work group

- decision on Issue 1: Yes or No
- decision on Issue 2: Yes or No. If Yes, Option 1 or Option 2.

## OTAI Modification process:
A spreadsheet is created to contained all the OTAI/LAI attributes. It can be used to tracking the proposed changes, i.e.,remove, add, modify on the existing version. 

1. Propose changes using the spreadsheet for discussion (all)
2. Change the OTAI spec document; (Weitang)
3. First official version of OTAI

Code change will be later or in parallel. 