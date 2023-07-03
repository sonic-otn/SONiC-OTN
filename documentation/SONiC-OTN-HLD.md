
# sonic-otn High Level Design

## Supported Optical Device Types
As there are different types of optical transport devices in optical network, some typical configurations are support as part of this project. Vendors should be able to customize their variations with minimum effort at device level under the same optical platform.
* Optical Amplifier: A terminal amplifier with protection is a target implementations. Non-protected or in-line amplifier can be customized from it. The optical diagram of terminal amplifier is shown as following:
<img src="../assets/ot-protected-amp.png" alt="SONiC for optical transport white-box system" style="zoom: 50%;" />

* 2D-ROADM: Single box support 2 degree ROAMD is another target implementation. High degree ROADM could be formed by multiple 2D-ROADM (disaggregated approach).
<img src="../assets/ot-2d-roadm.png" alt="SONiC for optical transport white-box system" style="zoom: 50%;" />

* Transponders:[TBD]

* Optical-packet device: Such as [Cassini](https://cdn.brandfolder.io/D8DI15S7/as/q3wkdg-476u4o-8wg0g7/Cassini_at_a_Glance_-_Telecom_Infra_Project.pdf)

## Optical Features Supported

sonic-ont north bound interface adopts openconfig transport yang models. [OTAI](../documentation/OTAI-v0.0.1.md) APIs are defined for optical vendors to implement vendor specific drivers (user space or kernel). The drivers are loaded at runtime as shared libraries.
<img src="../assets/ot-whitebox.png" alt="SONiC for optical transport white-box system" style="zoom: 50%;" />


### High Level Design
The design objective is to fully inherit existing SONiC architecture and infrastructure and seamlessly extending SONiC implementation with optical features without impacting on current switch platforms. The following diagram shows potential changes at system level. The modifications of existing SONiC are illustrated in red.

<img src="../assets/ot-software.png" alt="SONiC for optical transport white-box system" style="zoom: 50%;" />

The the following repository and container level design will be provided later, as the project progressing. Some highlights including:
* For pure optical transport devices with no switching functionality, dhcp-relay and teamd container would be disabled.
* lldp and bgp container will be retained to support L2/L3 protocol on DCN and OSC interfaces of optical devices.
* For pmon container, new platform drivers (python subclasses) will be implemented by whitebox vendor to manage generic hardware, i.e., chassis, control card, line card, PSU, fan and thermal sensors. It also manages the optical module's generic aspect (manufacturing info, firmware version) as  [platform components](https://github.com/sonic-net/SONiC/blob/master/doc/platform_api/new_platform_api.md).
* [openconfig optical transport yang models](https://github.com/openconfig/public/tree/master/release/models/optical-transport) are adopted to support optical features, thus to be added to the existing SONiC NBI (REST and CLI) in sonic-management-common and sonic-management-framework.
* In swss container, new optical configure managers will be added to process config change in config-db and update the app-db. Also ochagent will be enhanced to invoke OTAI APIs to modify the asic-db.
* Enhanced Syncd container processes the updates in asic-db and invoke OTAI APIs to interacting with optical modules. Whether OTAI communicated to optical module directly or via line card software stack is hardware/vendor dependent. As long as vendor implements the OTAI as library dynamically loaded at run time.
* sonic-buildimage repository will be enhanced by adding newly supported optical platforms. These platforms are parallel to the existing switch ASIC platforms, such as broadcom, barefoot, marvell and vs. 

In addition, an optical application container will be added to host optical monitoring and control processes including channel and port level power control loop and auto gain control by invoking NBI REST/RESTCONF APIs