
# sonic-otn HLD Discussion

## Supported Optical Device Types
As there are different types of optical transport devices in optical network, some typical configurations are support as part of this project. Vendors should be able to customize their variations with minimum effort at device level under the same optical platform.
* Optical Amplifier: A terminal amplifier with protection is a target implementations. Non-protected or in-line amplifier can be customized from it. The optical diagram of terminal amplifier is shown as following:
<img src="../../assets/ot-protected-amp.png" alt="SONiC for optical transport white-box system" style="zoom: 50%;" />

* 2D-ROADM: Single box support 2 degree ROAMD is another target implementation. High degree ROADM could be formed by multiple 2D-ROADM (disaggregated approach).
<img src="../../assets/ot-2d-roadm.png" alt="SONiC for optical transport white-box system" style="zoom: 50%;" />

* Transponders:[TBD]

* Optical-packet device: Such as [Cassini](https://cdn.brandfolder.io/D8DI15S7/as/q3wkdg-476u4o-8wg0g7/Cassini_at_a_Glance_-_Telecom_Infra_Project.pdf)

## Optical Features Supported

sonic-ont north bound interface adopts openconfig transport yang models. [OTAI](../documentation/OTAI-v0.0.1.md) APIs are defined for optical vendors to implement vendor specific drivers (user space or kernel). The drivers are loaded at runtime as shared libraries.
<img src="../../assets/ot-whitebox.png" alt="SONiC for optical transport white-box system" style="zoom: 50%;" />


### High Level Design
The design objective is to fully inherit existing SONiC architecture and infrastructure and seamlessly extending SONiC implementation with optical features without impacting on current switch platforms. The following diagram shows potential changes at system level. The modifications of existing SONiC are illustrated in red.

<img src="../../assets/ot-software.png" alt="SONiC for optical transport white-box system" style="zoom: 50%;" />

The the following repository and container level design will be provided later, as the project progressing. Some highlights including:
* For pure optical transport devices with no switching functionality, dhcp-relay and teamd container would be disabled.
* lldp and bgp container will be retained to support L2/L3 protocol on DCN and OSC interfaces of optical devices.
* For pmon container, new platform drivers (python subclasses) will be implemented by whitebox vendor to manage generic hardware, i.e., chassis, control card, line card, PSU, fan and thermal sensors. It also manages the optical module's generic aspect (manufacturing info, firmware version) as  [platform components](https://github.com/sonic-net/SONiC/blob/master/doc/platform_api/new_platform_api.md).
* [openconfig optical transport yang models](https://github.com/openconfig/public/tree/master/release/models/optical-transport) are adopted to support optical features, thus to be added to the existing SONiC NBI (REST and CLI) in sonic-management-common and sonic-management-framework.
* In swss container, new optical configure managers will be added to process config change in config-DB and update the app-DB. Also ochagent will be enhanced to invoke OTAI APIs to modify the asic-DB.
* Enhanced Syncd container processes the updates in asic-db and invoke OTAI APIs to interacting with optical modules. Whether OTAI communicated to optical module directly or via line card software stack is hardware/vendor dependent. As long as vendor implements the OTAI as library dynamically loaded at run time.
* sonic-buildimage repository will be enhanced by adding newly supported optical platforms. These platforms are parallel to the existing switch ASIC platforms, such as broadcom, barefoot, marvell and vs. 

In addition, an optical application container will be added to host optical monitoring and control processes including channel and port level power control loop and auto gain control by invoking NBI REST/RESTCONF APIs.

## Design Options
The sonic-otn HLD is still under review and discussion. There are three options presented so far. Here are the brief description of these three architectural options. The optical service related modules are the focus here, as the core part of optical NOS is to manage and control optical service. Network service core modules are SAI, swss, redis DB and syncd in SONiC. Related repos are ``sonic-swss, SAI and sonic-sairedis``. On theother hand, enhancement on NBI and generic hardware management, ``sonic-mgmt-framework, sonic-mgmt-common, sonic-platform-daemon, sonic-platform-common``, are relatively straight forward and can be discussed later.

To provide some background for discussion, the following diagram illustrats existing SONiC core containers to support packet switching functionality.

<img src="../../assets/arch-option-sonic.png" alt="SONiC for optical transport white-box system" style="zoom: 40%;" />

Please note that SAI API is implemented in both swss and syncd containers and the south bound interface(SBI) is conceptually provided by SAI in swss container, which updates Flex-counter DB and ASIC DB. By subscribing Flex-couter DB and ASIC DB changes, syncd will call invloking vendor SAI implementaion to communicate to hardware/ASIC.

### Option 1: New OTN Containers
Current sonic-otn implementation contributed by ALI/Accelink.

<img src="../../assets/arch-option1-ali-accelink.png" alt="SONiC for optical transport white-box system" style="zoom: 40%;" />

In this approach, original SAI, swss and sairedis is modified to support OTN functions. As a result, three new repos ``sonic-olss, OTAI and sonic-laredis`` are created. In these repos, new business logic for OTN is added and the build/compile and runtime infrastructure is cloned from SONiC and modified.

<b>pros</b>: 
- A working system. This is one of the first SONiC based optical NOS.
- Packet switching business logic code is removed and code simplified for OTN devices.

<b>cons</b>:
- created a parallel code base of SONiC infrastructure (scripts, utilities, data types, meta, code generation, etc.). This would make maintenance(continuously sync with upstream SONiC) a big effort.
- Potential support for packet-optical device is not feasible. 

<b>Option 1 Evolution plan proposal:</b>

On syncd side, since we copied the SONiC infrastructure in ot-Syncd and OTAI in Option 1, we can make these SONiC infrastructures as a common library.  mgmt-common, swss-common, pmon-common are common libraries in SONiC if more applications start to share these common libraries. It might be the first a new application start to share the SAI infrastructure and Syncd infrastructure, we can generate Syncd-common and SAI-common at day two when most infrastructure utilities are the same as SONiC's.

On the swss side, SONiC already separated infrastructure (swss-common lib) from the business-logic (swss). In most of time, developers are working on the OTN and Switch application layer instead of infrastructure (compared SWSS with SWSS-common).  We may avoid mixing the OTN and switch application service together in the same docker, in order to reusing these infrastructure. New olss container code mostly are business logic for OTN. Limited code duplication between swss and olss is worth to achieve a clear separation of switching and OTN functionality.

<img src="../../assets/arch-option1-evol.png" alt="SONiC for optical transport white-box system" style="zoom: 40%;" />

### Option 2: OTN Functions in Existing Containers
This option is proposed by Molex team based on deep dive studying of existing SONiC design and implementation, as well as extensive prototyping.

<img src="../../assets/arch-option2-molex.png" alt="SONiC for optical transport white-box system" style="zoom: 40%;" />

In this approach, original SAI, swss and sairedis are retained and unchanged. OTN code only contains the new APIs and optical business logic (OTAI, confmgr and orchagent). At compile time, the OTN code is plugged into SONiC code base (always latest) for OTN platforms, and OTN code is not included build for switching platforms. At runtime, the packet switching functions/features in swss, syncd will not be initialized/activated for a OTN platform. Therefore, syncd only uses OTAI APIs.

<b>pros</b>:
- Total transparent (no impact) for the existing switching platform, as the OTN code will not included in the image building.
- Since there are no duplicated code from existing SONiC code base, no on-going maintenance is needed, as upstream SONiC evolving. 
- set up a good base to support potential optical-packet device.

<b>cons</b>: 
- Compile time and run time dynamic behavior has certain technical risk, which has been reduced with some prototyping.
- For OTN platform, existing business logic code for packet switching is not used but is included the image.

### Option 3: New OTN Containers and Existing Containers
In this approach proposed by Infinera team, two new containers (olss, ot-syncd) are added while original swss and syncd for packet switch are retained at runtime. Also a new optical-DB is introduced to store optical info (equivalent to asic-DB).

<img src="../../assets/arch-option3-infinera.png" alt="SONiC for optical transport white-box system" style="zoom: 40%;" />

Assume that olss and ot-syncd are same as in option 1. The difference is this option uses new optical-DB, instead of ASIC-DB, for OTN.

<b>pros</b>:
- Total separation of OTN (code base and runtime container) from existing switching platform. 
- As switching features are available all the time optical-packet device can be easily supported. 

<b>cons</b>: 
- Same issue of duplicated code as in option 1. 
- Technical risk: (more investigation and prototyping are needed)
  - The current SONiC architecture does not support two swss and syncd container. 
  - For OTN devices, A dummy vendor sai need to be implemented. 

### Discussions
Option 1 and 3 are very close, except:
- Option 3 bring up original swss and synd containers.
- Option 3 added a new optical-DB.

Option 1 and 2 would converge eventually:
- Option 2 is designed to seamlessly extend SONiC with no duplicated infrastructure.
- In Option 1 Day 2 architecture, the same could be largely achieve, except common code between olss and swss.

## SONiC Containers
As SONiC OTN does not need many switching features, some docker containers are not used in SONiC OTN platforms. The following table listed changes required by OTN at container level. As most switch features are disabled, SONiC OTN is consider as light weight SONiC.
  
| Container/Feature Name | Is Enabled? |
| ---------------------- | ----------- |
| SNMP                   | Yes         |
| Resttapi               | Yes         |
| Telemetry              | Yes         |
| Database               | Yes         |
| Pmon                   | Yes         |
| Config-engine          | Yes         |
| Mgmt                   | Modify      |
| Syncd                  | Modify(ot-syncd) |
| Swss                   | Modify(olss)|
| LLDP                   | Optional    |
| BGP                    | Optional    |
| Teamd                  | No          |
| Nat                    | No          |
| Sflow                  | No          |
| DHCP Relay             | No          |
| Radv                   | No          |
| Macsec                 | No          |
| Mux                    | No          |
| PDE                    | No          |
| ICCPD                  | No          |
| P4RT                   | No          |


## Decision
After Evaluating the design option, option 1 is chosen and the tasks for SONiC repos are listed as followings. The goal is to merge and push the OTN code to upstream SONiC repos:
- new repos: OTAI, sonic-otairedis, sonic-olss. We should ask for feedback from SONiC maintainers (TSM) and accept these new repos. We may want to resync them with latest SAI, sonic-sairedis and sonic-swss first.
- For other respos changed and diverged, we should make them mergeable, not creating new repos just for OTN:
  - sonic-platform-daemon and sonic-platform-common are significantly changed for OTN. The original base classes and processes need to be restored. If new features are required by OTN, then enhancement can be submitted as PRs
   - sonic-management-common and sonic-management-framework: Add new yang, CV, and transformer/apps for Openconfig models. Should have no conflict.
  - sonic-swss-common: this is the core lib used by all containers (logging, db access etc..). OTN changes including, nameing, and removing original SONiC redis table etc.. The code need to be reverted into its original version and then apply OTN changes without impact existing switching platform
  - sonic-buildimage: ideally, we just add new OTN platforms/devices/drivers, so it is add-on code change and should be compatible.