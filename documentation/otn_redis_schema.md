# CONFIG_DB
## Platform
### FAN
*CLI*

```
key                 = FAN|FAN-<1-5>  ; string
;field               = value
speed_rate          = STRING
```

### LINE CARD

*openconfig-platform:components/component=LINECARD-1-<1-4>/linecard/config*

```
key                       = LINECARD|LINECARD-1-<1-4> ; string
;field                     = value
linecard-type             = STRING
board-mode                = STRING
temp-high-alarm-threshold = float64
temp-high-warn-threshold  = float64
temp-low-alarm-threshold  = float64
temp-low-warn-threshold   = float64
led-color                 = STRING           ; enum: GREEN, RED, YELLOW, RED_FLASH, GREEN_FLASH, RED_GREEN_FLASH, OFF, ABNORMAL
power-admin-state         = STRING           ; enum: ENABLED, DISABLED 
admin-state               = STRING           ; enum: ENABLED, DISABLED, MAINT
led-mode                  = STRING           ; enum: AUTO, FORCE_FLASH
led-flash-interval        = int           
```

### TRANSCEIVER
*openconfig-platform:components/component=TRANSCEIVER-1-1-C<1-12>/transceiver/config*

```
key                    = TRANSCEIVER|TRANSCEIVER-1-1-C<1-12>  ; string
;field                  = value
enabled                = "true"/ "false"      ; boolean
fec-mode               = STRING               ; enum: FEC_AUTO, FEC_DISABLED, FEC_ENABLED
vendor-expect          = STRING
power-mode             = STRING               ; enum: NORMAL, LOW_POWER
ethernet-pmd-preconf   = STRING               ; enum: ETH_100G_AOC, ETH_100G_ACC, ETH_100GBASE_SR10, ETH_100GBASE_SR4, ETH_100GBASE_LR4, ETH_100GBASE_ER4, ETH_100GBASE_CWDM4, ETH_100GBASE_CLR4, ETH_100GBASE_PSM4, ETH_100GBASE_CR4, ETH_100GBASE_FR, ETH_400GBASE_ZR, ETH_400GBASE_LR4, ETH_400GBASE_FR4, ETH_400GBASE_LR8, ETH_400GBASE_DR4, ETH_UNDEFINED, ETH_AUTO, ETH_100GBASE_4WDM10
```

### OCH

*openconfig-platform:components/component=OCH-1-1-L<1-2>/state*

```
key                 = OCH|OCH-1-1-L<1-2>  ; string
;field               = value
operational-mode    = STRING
frequency           = int
target-output-power = float64
```

## Terminal Device

### LOGICAL_CHANNEL
*openconfig-terminal-device:terminal-device/logical-channels*

```
key                         = LOGICAL_CHANNEL|CH<index>  ; string
;field                       = value
loopback-mode               = STRING         ; enum: NONE, FACILITY, TERMINAL
admin-state                 = STRING         ; enum: ENABLED, DISABLED, MAINT
link-down-delay-mode        = "true"/ "false"      ; boolean
link-down-delay-hold-off    = int
test-signal                 = "true"/ "false"      ; boolean
```

### OTN

*openconfig-terminal-device:terminal-device/logical-channels/channel/otn*

```
key                          = OTN|CH<index>  ; string
;field                        = value
tti-msg-auto                 = "true"/ "false"      ; boolean   
tti-msg-transmit             = STRING
tti-msg-expected             = STRING
delay-measurement-enabled    = "true"/ "false"      ; boolean
maintenance                  = STRING               ; enum: AIS, LCK, OCI
```

### ETHERNET

*openconfig-terminal-device:terminal-device/logical-channels/channel/ethernet*

```
key                     = ETHERNET|CH<index>  ; string
;field                   = value
client-als              = uint32
als-delay               = STRING      ; enum: NONE, LASER_SHUTDOWN, ETHERNET
maintenance             = STRING      ; enum: LF, RF, IDLE, NONE
client-rx-als-switch    = STRING      ; enum: LASER_SHUTDOWN, ETHERNET
```

### MODE

*openconfig-terminal-device:terminal-device/operational-modes/mode=25/state*

```
key                 = MODE|<mode-id>      ; string
;field               = value
vendor-id           = STRING
mode-id             = uint16
description         = STRING
```

## System

### DEVICE_METADATA

*/openconfig-system:system/state*

```
key                 = DEVICE_METADATA|localhost      ; string
;field               = value
hostname            = STRING
timezone-name?      = STRING
```

### LLDP

*openconfig-lldp*

```
key                 = LLDP|INTERFACE-1-<1-4>-1-OSC      ; string
;field               = value
enabled             = "true"/ "false"      ; boolean
name                = STRING  
```

## Telemetry

### DESTINATION_GROUP

*/openconfig-telemetry:telemetry-system/destination-groups/destination-group*

```
key                 = DESTINATION_GROUP|collector_dest_group      ; string
;field               = value
group-id            = STRING
```

### DESTINATION

*/openconfig-telemetry:telemetry-system/destination-groups/destination-group/destinations/destination*

```
key                 = DESTINATION|collector_dest_group|1.1.1.1|15555      ; string
;field               = value
destination-address = STRING
destination-port    = uint16
```

### SENSOR_GROUP

*/openconfig-telemetry:telemetry-system/sensor-groups/sensor-group*

```
key                 = SENSOR_GROUP|ethernet_sensor_group      ; string
;field               = value
sensor-group-id     = STRING
sensor-paths        = STRING
```

### PERSISTENT_SUBSCRIPTION

*/openconfig-telemetry:telemetry-system/subscriptions/persistent-subscriptions/persistent-subscription*

```
key                 = PERSISTENT_SUBSCRIPTION|XXX_terminal_sub_v100      ; string
;field               = value
name                = STRING
encoding            = STRING     ; enum: ENC_JSON_IETF:1, ENC_PROTO3:2, ENC_XML:3
protocol            = STRING     ; enum: STREAM_GRPC:1, STREAM_JSON_RPC:2, STREAM_SSH:3, STREAM_THRIFT_RPC:4, STREAM_WEBSOCKET_RPC:5
```

### SUBSCRIPTION_DESTINATION_GROUP

*/openconfig-telemetry:telemetry-system/subscriptions/persistent-subscriptions/persistent-subscription/destination-groups/destination-group*

```
key                 = SUBSCRIPTION_DESTINATION_GROUP|XXX_terminal_sub_v100|collector_dest_group      ; string
;field               = value
group-id            = STRING
```

### SENSOR_PROFILE

*/openconfig-telemetry:telemetry-system/subscriptions/persistent-subscriptions/persistent-subscription/sensor-profiles/sensor-profile*

```
key                 = SENSOR_PROFILE|XXX_terminal_sub_v100|alarm_sensor_group      ; string
;field               = value
sensor-group        = STRING
heartbeat-interval  = uint64
sample-interval     = uint64
```

## Optical Line System

### AMPLIFIER

*;/openconfig-optical-amplifier:optical-amplifiers/amplifier/config*

*;revision "2019-12-06" {reference "0.5.0"}*

```
key                 = AMPLIFIER|name         ; string
;feild              = value
type                = STRING                 ; identityref: EDFA, FORWARD_RAMAN,  BACKWARD_RAMAN, HYBRID
target-gain         = float64                ; yang decimal64, json Number
max-gain            = float64
min-gain            = float64
target-gain-tilt    = float64
gain-range          = STRING                 ; identityref: LOW_GAIN_RANGE, MID_GAIN_RANGE, HIGH_GAIN_RANGE, FIXED_GAIN_RANGE
amp-mode            = STRING                 ; identityref: CONSTANT_GAIN, CONSTANT_POWER, DYNAMIC_GAIN
target-output-power = float64
max-output-power    = float64
enabled             = "true" / "false"       ; boolean
fiber-type-profile  = STRING                 ; identityref: DSF, LEAF, SSMF, TWC, TWRS
```

### APS

*/openconfig-transport-line-protection*

```
key                 = APS|<name>            ; string
;field               = value
name                = STRING
force-to-port       = STRING
```

### ATTENUATOR

*;/openconfig-optical-attenuator:optical-attenuators/attenuator/config*

*;revision "2019-07-19" {reference "0.1.0"}*

```
key                 = ATTENUATOR|<name>      ; string
;field               = value
attenuation-mode    = STRING                 ; identityref
target-output-power = float64                ; yang decimal64, json Number
attenuation         = float64
enabled             = "true" / "false"       ; boolean
```

### MEDIA_CHANNEL

*/openconfig-wavelength-router*

```
key                      = MEDIA_CHANNEL|<index>      ; string
;field                    = value
name                     = STRING
lower-frequency          = uint64
upper-frequency          = uint64
admin-status             = STRING           ; enum: ENABLED, DISABLED, MAINT
super-channel            = "true"/ "false"  ; boolean
super-channel-parent     = uint32
attenuation-control-mode = STRING           ; enum: ATTENUATION_FIXED_LOSS, ATTENUATION_DYNAMIC_LOSS, ATTENUATION_DYNAMIC_LOSS_DAMPED
source-port-name         = STRING
dest-port-name           = STRING
ase-control-mode    = STRING              ; identityref
  ;ASE_ENABLED, ASE_DISABLED, AUTO_ASE_ON_FAILURE, AUTO_ASE_FAILURE_AND_RESTORE
ase-injection-thrshold    = float64
wait-to-restore-time = uint32 ; otn (alibaba) extension
```

### MEDIA_CHANNEL_DISTRIBUTION
```
key                  = MEDIA_CHANNEL|<index>|<lower-frequency>|<upper-frequency
;field    = value
target-power         = float64
attenuation-value    = float64   ; otn (alibaba) extension
```

### OCM

*;/openconfig-channel-monitor:channel-monitors/channel-monitor/config*

*;revision "2019-10-24" {reference "0.4.0"}*

```
key                 = OCM|name ;leafref to platform component (OCM-1-<1-8>-<1-n>)
;field               = value
monitor-port        = STRING   ;leafref to platform component
```

### OTDR

*No openconfig yang*

```
key                    = OTDR|OTDR-1-<1-8>-<1-n>      ; string
;field                  = value
name                   = STRING
parent-port            = STRING
refractive-index       = float64
backscatter-index      = float64
reflection-threshold   = float64
splice-loss-threshold  = float64
end-of-fiber-threshold = float64
distance-range         = uint32
pulse-width            = uint32
average-time           = uint32
output-frequency       = uint64
enable                 = "true"/ "false"      ; boolean
start-time             = STRING
period                 = uint32
```

## Counter Config

### FLEX_COUNTER_TABLE

*sonic*

```
key                 = FLEX_COUNTER_TABLE|OTN_COUNTER_ID_LIST_GAUGE      ; string
;field               = value
attr1               = STRING
```

### FLEX_COUNTER_GROUP_TABLE

*sonic*
```
key                 = FLEX_COUNTER_GROUP_TABLE|1S_STAT_GAUGE      ; string
;field               = value
FLEX_COUNTER_STATUS ?=
POLL_INTERVAL       ?=
```

### ALARMPROFILE

*CLI*

```
key                 = ALARMPROFILE|<alarm_type-id>     ; string
;field               = value
severity            = STRING    ; enum: CRITICAL, MAJOR, MINOR, UNKNOWN, WARNING
```

# STATE_DB

## Platform

### FAN

*openconfig-platform:components/component=FAN-1-<7-11>/state*

```
key                 = FAN|FAN-1-<7-11>  ; string
;field               = value
serial-no           = STRING
part-no             = STRING
speed-rate          = STRING ; 1-9 | auto
mfg-name            = STRING
mfg-date            = STRING
hardware-version    = STRING
oper-status         = STRING
empty               = "true"/ "false"   ; boolean
removable           = "true"/ "false"   ; boolean
parent              = STRING
```

### LED

*openconfig-platform:components/component=FAN-1-<7-11>/state/leds/led*

*openconfig-platform:components/component=PSU-1-<5-6>/state/leds/led*

*openconfig-platform:components/component=CU-1/state/leds/led*

*openconfig-platform:components/component=PORT-1-1-(C<1-12>, L<1-2>)/state/leds/led*

*openconfig-platform:components/component=LINECARD-1-<1-4>/state/leds/led*
```
key                 = LED|PSU-1-<5-6>|name    ; string
;field               = value
name                = STRING
led-status          = STRING                  ; enum: RED, GREEN, RED_FLASH, GREEN_FLASH, RED_GREEN_FLASH, OFF, ABNORMAL
led-function        = STRING
```


### PSU

*openconfig-platform:components/component=PSU-1-<5-6>/state*
```
key                 = PSU|PSU-1-<5-6>  ; string
;field               = value
serial-no           = STRING
part-no             = STRING
capacity            = float64
enabled             = "true"/ "false"      ; boolean
mfg-name            = STRING 
mfg-date            = STRING 
hardware-version    = STRING 
oper-status         = STRING 
empty               = "true"/ "false"      ; boolean
removable           = "true"/ "false"      ; boolean
parent              = STRING
```
### CU

*openconfig-platform:components/component=CU-1/state*

```
key                 = CU|CU-1  ; string
;field               = value
serial-no           = STRING
part-no             = STRING
mfg-name            = STRING
mfg-date            = STRING
hardware-version    = STRING
oper-status         = STRING
empty               = "true"/ "false"    ;boolean
removable           = "true"/ "false"    ;boolean
parent              = STRING
memory-available    = uint64
memory-utilized     = uint64
software-version    = STRING
```

### CHASSIS

*openconfig-platform:components/component=CHASSIS-1/state*
```
key                 = CHASSIS|CHASSIS-1  ; string
;field               = value
oper-status         = STRING
```

### LINECARD

*openconfig-platform:components/component=LINECARD-1-<1-4>/state*
```
key                 = LINECARD|LINECARD-1-<1-4>  ; string
;field               = value
board-mode          = STRING        ;enum = LA_400G_CA_200GE, L1_400G_CA_100GE, L2_400G_CA_100GE, LA_400G_RE_200GE, LA_400G_SNCP_CA_100GE, LA_400G_SNCP_C12_200GE, LA_400G_SNCP_C34_200GE, LA_400G_RE_100GE
linecard-type       = STRING
power-admin-state   = STRING
```

### TRANSCEIVER_C

*openconfig-platform:components/component=TRANSCEIVER-1-1-C<1-12>/transceiver/state*

```
key                 = TRANSCEIVER|TRANSCEIVER-1-1-C<1-12>  ; string
;field               = value
vendor              = STRING
vendor-expect       = ?enum
vendor-rev          = STRING
date-code           = STRING
vendor-part         = STRING
fec-mode            = STRING        ;enum : FEC_ENABLED
form-factor         = STRING        ;enum : QSFP28
ethernet-pmd        = STRING        ;enum : ETH_100GBASE_CWDM4
present             = STRING        ;enum : PRESENT, NOT_PRESENT
enabled             = "true"/ "false"    ;boolean
fault-condition     = "true"/ "false"    ;boolean
connector-type      = STRING        ;enum : LC_CONNECTOR
part-no             = STRING
extend-module-code  = STRING        ;enum : SDR, DDR, QDR, FDR, EDR
data-status         = STRING        ;enum : NOT_READY, READY
encode              = STRING        ;enum : UNSPECIFIED, 8B_10B, 4B_5B, NRZ, SONET_SCRAMBLED, 64B_66B, MANCHESTER, 256B_257B, PAM4
bitrate              = double
extend-bit-rate      = double
wavelength           = double
wavelength-tolerance = double
oui                  = STRING
rn                   = STRING
identifier           = STRING
cmis-revision        = STRING
voltage-alarm-threshold  = double
rx-power-alarm-threshold = double 
tx-power-alarm-threshold = double
tx-bais-alarm-threshold  = double
```

### PHYSICAL_CHANNEL
*openconfig-platform:components/component=TRANSCEIVER-1-1-C<1-12>/transceiver/physical-channels/channel=<1-4>/state*
*openconfig-platform:components/component=TRANSCEIVER-1-1-L<1-2>/transceiver/physical-channels/channel=1/state*

```
key                 = TRANSCEIVER|TRANSCEIVER-1-1-C<1-12>|CH-<1-4>  ; string
;field               = value
output-frequency    = uint64
tx-laser            = "true"/ "false"    ;boolean 
```

### TRANSCEIVER_L
*openconfig-platform:components/component=TRANSCEIVER-1-1-L<1-2>/transceiver/state*

```
key                 = TRANSCEIVER|TRANSCEIVER-1-1-L<1-2>  ; string
;field               = value
vendor              = STRING
vendor-rev          = STRING
date-code           = STRING
vendor-part         = STRING
fec-mode            = STRING             ;enum : FEC_ENABLED, FEC_DISABLED, FEC_AUTO
form-factor         = STRING             ;enum : QSFP28
ethernet-pmd        = STRING             ;enum : ETH_100GBASE_CWDM4
present             = STRING             ;enum : PRESENT, NOT_PRESENT
enabled             = "true"/ "false"    ;boolean
fault-condition     = "true"/ "false"    ;boolean
connector-type      = STRING             ;enum : AOC_CONNECTOR, DAC_CONNECTOR, LC_CONNECTOR, MPO_CONNECTOR, SC_CONNECTOR
part-no             = STRING
laser-age           = int
operate-time        = int
lot-code            = STRING
clei-code           = STRING
power-class         = STRING             ;enum : BELOW_9W, BELOW_12W, BELOW_15W, ABOVE_15W
newwork-bit-rate    = double
host-bit-rate       = double
sm-fiber-len        = double
mm-fiber-len        = double
copper-cable-len    = double
max-wavelength      = double
min-wavelength      = double
max-tx-power                            = double
max-rx-power                            = double
max-oper-temp                           = double
min-oper-temp                           = double
vcc-high-alarm-threshold                = double
vcc-high-warn-threshold                 = double
vcc-low-alarm-threshold                 = double
vcc-low-warn-threshold                  = double
rx-total-power-high-alarm-threshold     = double
rx-total-power-high-warn-threshold      = double
rx-total-power-low-alarm-threshold      = double
rx-total-power-high-alarm-threshold     = double
oa-pump-current-high-alarm-threshold    = double
oa-pump-current-high-warn-threshold     = double
oa-pump-current-low-alarm-threshold     = double
oa-pump-current-low-warn-threshold      = double
tx-bais-high-alarm-threshold            = double
tx-bais-high-warn-threshold             = double
tx-bais-low-alarm-threshold             = double
tx-bais-low-warn-threshold              = double
```

### PORT

*openconfig-platform:components/component=PORT-1-1-(C<1-12>, L<1-2>)/state*

*openconfig-platform:components/component=PORT-1-<1-4>-MD<33,38,43,48,53,58,EXP><IN|OUT>/state*

*openconfig-platform:components/component=PORT-1-<1-4>-EXP<IN|OUT>/state*

*openconfig-platform:components/component=PORT-1-<1-4>-L<1-2><IN|OUT>/state*

*openconfig-platform:components/component=PORT-1-<1-4>-<1-2>-EDFA<IN|OUT>/state*

*openconfig-platform:components/component=PORT-1-<1-4>-OLP<PRI,SEC,COM><IN|OUT>/state*

```
key                 = PORT|PORT-1-1-(C<1-12>, L<1-2>)  ; string
;field               = value
admin-state         = STRING        ; enum : enabled, disabled
oper-status         = STRING        ; enum : ACTIVE, INACTIVE, DISABLE
optical-port-type   = STRING    ; enum: ADD, DROP, EGRESS, INGRESS, MONITOR, TERMINAL_CLIENT, TERMINAL_LINE
```

### OCH

*openconfig-platform:components/component=OCH-1-1-L<1-2>/state*
```
key                 = OCH|OCH-1-1-L<1-2>  ; string
;field               = value
operational-mode    = STRING
target-output-power = float64
frequency           = uint64
line-port           = STRING
oper-status         = STRING
parent              = STRING
```

## Terminal Device 

### LOGICAL_CHANNEL

*openconfig-terminal-device:terminal-device/logical-channels/channel=<index>/state*

```
key                     = LOGICAL_CHANNEL|CH<index>  ; string
;field                   = value
index                   = uint32
description             = STRING
admin-state             = STRING            ; enum: ENABLED, DISABLED, MAINT
rate-class              = STRING            ; enum: TRIB_RATE_1000G, TRIB_RATE_100G, TRIB_RATE_1050G, TRIB_RATE_10G, TRIB_RATE_1100G, TRIB_RATE_1150G, TRIB_RATE_1200G, TRIB_RATE_1250G, TRIB_RATE_1300G, TRIB_RATE_1350G, TRIB_RATE_1400G, TRIB_RATE_1450G, TRIB_RATE_1500G, TRIB_RATE_150G, TRIB_RATE_1550G, TRIB_RATE_1600G, TRIB_RATE_1G, TRIB_RATE_2.5G, TRIB_RATE_200G, TRIB_RATE_250G, TRIB_RATE_300G, TRIB_RATE_350G, TRIB_RATE_400G, TRIB_RATE_40G, TRIB_RATE_450G, TRIB_RATE_500G, TRIB_RATE_550G, TRIB_RATE_600G, TRIB_RATE_650G, TRIB_RATE_700G, TRIB_RATE_750G, TRIB_RATE_800G, TRIB_RATE_850G, TRIB_RATE_900G, TRIB_RATE_950G
trib-protocol           = STRING            ; enum: PROT_100GE, PROT_100G_MLG, PROT_10GE_LAN, PROT_10GE_WAN, PROT_1GE, PROT_400GE, PROT_40GE, PROT_OC192, PROT_OC48, PROT_OC768, PROT_ODU2, PROT_ODU2E, PROT_ODU3, PROT_ODU4, PROT_ODUCN, PROT_ODUFLEX_CBR, PROT_ODUFLEX_GFP, PROT_OTSIG, PROT_OTU1E, PROT_OTU2, PROT_OTU2E, PROT_OTU3, PROT_OTU4, PROT_OTUCN, PROT_STM16, PROT_STM256, PROT_STM64
logical-channel-type    = STRING            ; enum: PROT_ETHERNET, PROT_OTN
loopback-mode           = STRING            ; enum: NONE, FACILITY, TERMINAL
test-signal             = "true"/ "false"   ; boolean 
link-state              = STRING            ; enum: UP,DOWN, TESTING
transceiver             = STRING
```

### OTN

*openconfig-terminal-device:terminal-device/logical-channels/channel=<index>/otn/state*

```
key                          = OTN|CH<index>  ; string
;field                        = value
index                        = uint32
tti-msg-transmit             = STRING
tti-msg-expected             = STRING
tti-msg-auto                 = "true"/ "false"      ; boolean   
delay-measurement-enabled    = "true"/ "false"      ; boolean
delay-measurement-mode       = STRING               ; enum: LOOPBACK, MEASURE
tti-msg-recv                 = STRING
rdi-msg                      = STRING
```

### ASSIGNMENT

*openconfig-terminal-device:terminal-device/logical-channels/channel=<index>/logical-channel-assignments/assignment=1/state*

```
key                          = ASSIGNMENT|CH<index>|ASS<index>  ; string
;field                        = value
index                        = uint32
description                  = STRING
assignment-type              = STRING                ; enum: LOGICAL_CHANNEL, OPTICAL_CHANNEL
logical-channel              = uint32
optical-channel              = STRING
allocation                   = float64
tributary-slot-index         = uint32
```

### ETHERNET

*openconfig-terminal-device:terminal-device/logical-channels/channel=<index>/ethernet/state*

```
key                          = ETHERNET|CH<index>  ; string
;field                        = value
index                        = uint32
client-als                   = uint32
als-delay                    = STRING     ; enum: NONE, LASER_SHUTDOWN, ETHERNET
```

### LLDP

*openconfig-terminal-device:terminal-device/logical-channels/channel=<index>/ethernet/lldp/state*

```
key                          = LLDP|CH<index>  ; string
;field                        = value
index                        = uint32
enabled                      = "true"/ "false"      ; boolean
snooping                     = "true"/ "false"      ; boolean
```

### NEIGHBOR

*openconfig-terminal-device:terminal-device/logical-channels/channel=<index>/ethernet/lldp/neighbors/neighbor=1/state*
```
key                          = NEIGHBOR | CH<index>|<id>  ; string
;field                        = value
id                           = uint32
system-name                  = STRING
system-description           = STRING
chassis-id                   = STRING
chassis-id-type              = STRING            ; enum: CHASSIS_COMPONENT, INTERFACE_ALIAS, PORT_COMPONENT, MAC_ADDRESS, NETWORK_ADDRESS, INTERFACE_NAME, LOCAL
id                           = STRING            ; different source ?
age                          = uint64
last-update                  = int64             ; ?
ttl                          = uint16
port-id                      = STRING
port-id-type                 = STRING            ;enum: INTERFACE_ALIAS, INTERFACE_ALIAS, PORT_COMPONENT, MAC_ADDRESS, NETWORK_ADDRESS, INTERFACE_NAME, LOCAL
port-description             = STRING
management-address           = STRING
management-address-type      = STRING
```

## System

### CURALARM

*openconfig-system:system/alarms/alarm=<alarm_id>*
```
key                 = CURALARM|<alarm_id>  ; string
;field               = value
id                  = STRING
time-created        = int
resource            = STRING
text                = STRING
severity            = STRING            ; enum: CRITICAL, MAJOR, MINOR, UNKNOWN, WARNING
type-id             = STRING            ; enum: AIS, EQPT, LOS, OTS
service-affect      = "true"/ "false"   ; boolean
```

### INTERFACE

*openconfig-interfaces:interfaces/interfaces/interface/state*

```
key                 = INTERFACE|<name> ;INTERFACE-1-1-C<1-12>, INTERFACE-1-<1-4>-1-OSC, INTERFACE-MGMT<1-2>
;field               = value
transceiver         = STRING
type                = STRING             ; enum: ethernetCsmacd
hardware-port       = STRING
name                = STRING
```

### LLDP

*openconfig-lldp*

```
key                          = LLDP|INTERFACE-1-<1-4>-1-OSC  ; string
;field                        = value
name                         = STRING
enabled                      = "true"/ "false"      ; boolean
snooping                     = "true"/ "false"      ; boolean
neighbor-id                  = STRING
neighbor-system-name         = STRING
neighbor-system-description  = STRING
neighbor-chassis-id          = STRING
neighbor-chassis-id-type     = STRING               ; enum: CHASSIS_COMPONENT, INTERFACE_ALIAS, PORT_COMPONENT, MAC_ADDRESS, NETWORK_ADDRESS, INTERFACE_NAME, LOCAL
neighbor-last-update         = int64                ; ?
neighbor-port-id             = STRING
neighbor-port-id-type        = STRING               ; enum: INTERFACE_ALIAS, INTERFACE_ALIAS, PORT_COMPONENT, MAC_ADDRESS, NETWORK_ADDRESS, INTERFACE_NAME, LOCAL
neighbor-port-description    = STRING
neighbor-management-address  = STRING
neighbor-management-address-type = STRING
```

### NTP

*openconfig-system:system/ntp/state*

```
key                 = NTP|localhost  ; string
;field               = value
enabled             = "true"/ "false"      ; boolean
ntp-source-address  = STRING
enable-ntp-auth     = "true"/ "false"      ; boolean
auth-mismatch       = uint64
ntp-server-selected = STRING
ntp-status          = STRING               ; enum: USET:0, UNREACHABLE:1, UNSYNCHRONIZED:2, SYNCHRONIZED:3
```

## Optical Line System

### Inventory data
*openconfig-platform.yang/components/component*

```
OC_COMPONENT_TABLE:
key                 = OC_COMPONET_TABLE|name  ; string 
;field               = value
serial-no           = STRING
part-no             = STRING
mfg-name            = STRING
mfg-date            = STRING
oper-status         = STRING
empty               = "true"/ "false"      ; boolean
removable           = "true"/ "false"      ; boolean
location            = STRING
parent              = STRING
subcomponents       = STRING
equipment-failure   = "true"/ "false"      ; boolean
equipment-mismatch  = "true"/ "false"      ; boolean
firmware-version    = STRING
hardware-version    = STRING
software-version    = STRING

```
name could be the name (key) from optical module tables (AMPLIFIER, OSC, WSS etc..):
OSC-1-1-1, OA-1-1, WSS-1-2 etc..

### APS

*openconfig-transport-line-protection*

```
key                 = APS|<name>  ; string
;field               = value
name                = STRING
revertive           = "true"/ "false"      ; boolean
wait-to-restore-time= uint32
hold-off-time       = uint32
primary-switch-threshold = float64
primary-switch-hysteresis = float64
secondary-switch-threshold = float64
relative-switch-threshold = float64
relative-switch-threshold-offset = float64
force-to-port             = STRING                ; enum: NONE:0, PRIMATRY:1, SENCONDARY:2
active-path               = STRING                ; enum: PRIMATRY:0, SENCONDARY:1
```

### APS_PORT

```
key                 = APS_PORT|<name>   ;  <name>_LinePrimaryIn, <name>_LinePrimaryOut, <name>_LineSecondaryIn, <name>_LineSecondaryOut, <name>_CommonIn, <name>_CommonOut 
;field               = value
enabled             = "true"/ "false"             ; boolean
target-attenuation  = float64  
attenuation         = float64  
```

### AMPLIFIER_TABLE

*;/openconfig-optical-amplifier:optical-amplifiers/amplifier/state*

*;revision "2019-12-06" {reference "0.5.0"}*

```
key                 = AMPLIFIER_TABLE|<name>      ; string
;field               = value
name                = STRING
type                = STRING    ; identityref: EDFA, FORWARD_RAMAN,  BACKWARD_RAMAN, HYBRID
target-gain         = float64   ; yang decimal64, json Number
min-gain            = float64   
max-gain            = float64  
target-gain-tilt    = float64 
gain-range          = STRING    ; identityref: LOW_GAIN_RANGE, MID_GAIN_RANGE, HIGH_GAIN_RANGE, FIXED_GAIN_RANGE
amp-mode            = STRING    ; identityref: CONSTANT_POWER, DYNAMIC_GAIN
target-output-power = float64
max-output-power    = float64
enabled             = "true"/ "false"      ; boolean
fiber-type-profile  = STRING    ; identityref: DSF, LEAF, SSMF, TWC, TWRS

component           = STRING    ; ref to platform component
ingress-port        = STRING    ; ref to platform component
egress-port         = STRING    ; ref to platform component
actual-gain         = float64   ; yang decimal64 (instant value only)
actual-gain-tilt    = float64 
input-power-total   = float64
input-power-c-band  = float64
input-power-l-band  = float64
output-power-total   = float64
output-power-c-band = float64
output-power-l-band = float64
laser-bias-current  = float64
optical-return-loss  = float64

```

### OSC_TABLE

*;/openconfig-optical-amplifier:optical-supervisory-channels/supervisory-channel/config*

*;revision "2019-12-06" {reference "0.5.0"}*

```
key                 = OSC_TABLE|interface  ; string (OSC-1-<1-4>-1)
;field               = value
input-power         = float64
output-power        = float64
laser-bias-current  = float64
output-frequency    = float64
```

### ATTENUATOR_TABLE
*;/openconfig-optical-attenuator:optical-attenuators/attenuator/state*

*;revision "2019-07-19" {reference "0.1.0"}*

```
key                 = ATTENUATOR_TABLE|<name>    ; string
;field               = value
attenuation-mode    = STRING           ; identityref: CONSTANT_POWER, CONSTANT_ATTENUATION
target-output-power = float64
attenuation         = float64
enabled             = "true"/ "false"  ; boolean
component           = STRING           ; ref to platform component
ingress-port        = STRING           ; ref to platform component
egress-port         = STRING           ; ref to platform component
actual-attenuation  = float64          ; yang decimal64 (instant value only)
output-power-total  = float64
optical-return-loss  = float64

```

### WSS

*openconfig-wavelength-router*
```
key                 = WSS|WSS-1-<1-4>-<1-n> ; string
;field               = value

```

### MEDIA_CHANNEL_TABLE

*openconfig-wavelength-router*

```
key                 = MEDIA_CHANNEL|<index>    ; string
;field               = value
index               = uint32
name                = STRING
lower-frequency     = uint64
upper-frequency     = uint64
admin-status        = STRING                ; enum: ENABLED, DISABLED, MAINT
super-channel       = "true"/ "false"      ; boolean
super-channel-parent= uint32
ase-control-mode    = STRING                ; enum: ASE_ENABLED, ASE_DISABLED, AUTO_ASE_ON_FAILURE, AUTO_ASE_FAILURE_AND_RESTORE temperary not used
ase-injection-mode  = STRING                ; enum: MODE_THRESHOLD, MODE_DELTA temperary not used
ase-injection-threshold=float64             ; temperary not used
ase-injection-delta    =float64             ; temperary not used
attenuation-control-mode = STRING           ; enum: ATTENUATION_FIXED_LOSS, ATTENUATION_DYNAMIC_LOSS, ATTENUATION_DYNAMIC_LOSS_DAMPED
source-port-name    = STRING
dest-port-name      = STRING
oper-status         = STRING                ; enum: UP, DOWN
ase-status          = STRING                ; enum: PRESENT, NOT_PRESENT
wait-to-restore-time= uint32
```

### MEDIA_CHANNEL_DISTRIBUTION_TABLE
```
key                  = MEDIA_CHANNEL|<index>|<lower-frequency>|<upper-frequency
;field    = value
target-power         = float64
attenuation-value    = float64   ; otn (alibaba) extension
```

### OCM_TABLE

*;/openconfig-channel-monitor:channel-monitors/channel-monitor/state*

*;revision "2019-10-24" {reference "0.4.0"}*

```
key                 = OCM_TABLE|name  ; string
;feild              = value
monitor-port        = STRING          ; leafref to platform component
```

### OCM_CHANNEL
*;/openconfig-channel-monitor:channel-monitors/channel-monitor/channels*

*;revision "2019-10-24" {reference "0.4.0"}*

```
key = OCM_CHANNEL|name|lower-frequency|upper-frequency
                       ; name is key in OCM_TABLE
;feild              = value
power               = float64             ; yang decimal64
```
### OTDR

*No openconfig yang*

```
key                 = OTDR|OTDR-1-<1-8>-<1-n>   ; string
;field               = value
name                = STRING
parent-port         = STRING
refractive-index    = float64
backscatter-index   = float64
reflection-threshold= float64
splice-loss-threshold = float64
end-of-fiber-threshold = float64
distance-range      = uint32
pulse-width         = uint32
average-time        = uint32
output-frequency    = uint64
enable              = "true"/ "false"      ; boolean
start-time          = STRING
period              = uint32
dynamic-range       = uint32
distance-accuracy   = float64
sampling-resolution = float64
loss-dead-zone      = float64
reflection-dead-zone= float64
scanning-status     = STRING               ; enum: ACTIVE, INACTIVE, DISABLE
serial-no           = STRING
part-no             = STRING
mfg-name            = STRING
mfg-date            = STRING
hardware-version    = STRING
firmware-version    = STRING
oper-status         = STRING
empty               = "true"/ "false"      ; boolean
removable           = "true"/ "false"      ; boolean
parent              = STRING
software-version    = STRING
```

*No openconfig yang*

```
key                 = OTDR|OTDR-1-<1-8>-<1-n>|BASELINE/CURRENT/<scan-time>  ; string
;field               = value
scan-time           = STRING
span-distance       = float64
span-loss           = float64
distance-range      = uint32
pulse-width         = uint32
average-time        = uint32
output-frequency    = uint64
update-time         = STRING
data                = STRING
```

### OTDR_EVENT

```
key                 = OTDR_EVENT|OTDR-1-<1-8>-<1-n>|BASELINE/CURRENT/<scan-time>|<index>  ; string
;field               = value
index               = uint32
length              = float64
loss                = float64
accumulate-loss     = float64
type                = STRING            ; enum: START, END, REFLECTION, NON-REFLECTION, FIBER-SECTION, UNKOWN
reflection          = float64
```

## Operations

### REBOOT

*CLI*

```
key                 = REBOOT|<entity-name>  ; string
;field               = value
entity-name         = STRING
reboot-type         = STRING
```

### CLEANPM

*CLI*
```
key                 = CLEANPM|<entity-name>  ; string
;field               = value
entity-name         = STRING
period              = STRING                 ; enum: 15, 24
```


# COUNTERS_DB, index 2
The OTN current PM data (gauge and counters statistics) is stored in COUNTER_DB.

### Gauge statistic table template
```
key                       = <table_name>:<object_index>_<PM_item_name>:<15_pm_current|24_pm_current> ; string  
;field                    = value  
interval                  = int              ; 900000000000 for 15 minutes interval,86400000000000 for 24 hours interval  
starttime                 = int              ; timestamp in nanoseconds relative to the Unix Epoch  
max                       = int | float64    ; float64 with fraction-digits 1, 2, and 18   
max-time                  = int              ; timestamp in nanoseconds relative to the Unix Epoch  
min                       = int | float64    ; float64 with fraction-digits 1, 2, and 18  
min-time                  = int              ; timestamp in nanoseconds relative to the Unix Epoch  
instant                   = int | float64    ; float64 with fraction-digits 1, 2, and 18  
avg                       = int | float64    ; float64 with fraction-digits 1, 2, and 18  
validity                  = STRING           ; enum: incomplete, complete, invalid  
```
          
#### LINECARD
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| LINECARD | LINECARD-1-<slot_index> | CpuUtilization | openconfig-platform/components/component/cpu/utilization/state  |   |
| LINECARD | LINECARD-1-<slot_index> | MemoryAvailable | openconfig-platform/components/component/state/memory/available  |   |
| LINECARD | LINECARD-1-<slot_index> | MemoryUtilized | openconfig-platform/components/component/state/memory/utilized   |   |
| LINECARD | LINECARD-1-<slot_index> | Temperature | openconfig-platform/components/component/state/temperature  |   |
    
  
#### OCH
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| OCH | OCH-1-<slot_index>-L<line_index> | CarrierFrequencyOffset |  openconfig-platform/components/component/oc-opt-term:optical-channel/state/carrier-frequency-offset |   |
| OCH | OCH-1-<slot_index>-L<line_index> | ChromaticDispersion | openconfig-platform/components/component/oc-opt-term:optical-channel/state/chromatic-dispersion  |   |
| OCH | OCH-1-<slot_index>-L<line_index> | EdfaBiasCurrent | vendor extensions  |   |
| OCH | OCH-1-<slot_index>-L<line_index> | GroupDelay |  vendor extensions |   |
| OCH | OCH-1-<slot_index>-L<line_index> | InputPower | openconfig-platform/components/component/oc-opt-term:optical-channel/state/input-power  |   |
| OCH | OCH-1-<slot_index>-L<line_index> | InputSignalPower | vendor extensions  |   |
| OCH | OCH-1-<slot_index>-L<line_index> | LaserBiasCurrent | openconfig-platform/components/component/oc-opt-term:optical-channel/state/laser-bias-current  |   |
| OCH | OCH-1-<slot_index>-L<line_index> | Osnr |  openconfig-platform/components/component/oc-opt-term:optical-channel/state/osnr |   |
| OCH | OCH-1-<slot_index>-L<line_index> | OutputPower |  openconfig-platform/components/component/oc-opt-term:optical-channel/state/output-power |   |
| OCH | OCH-1-<slot_index>-L<line_index> | PolarizationDependentLoss | openconfig-platform/components/component/oc-opt-term:optical-channel/state/polarization-dependent-loss  |   |
| OCH | OCH-1-<slot_index>-L<line_index> | SecondOrderPolarizationModeDispersion |  openconfig-platform/components/component/oc-opt-term:optical-channel/state/second-order-polarization-mode-dispersion |   |
| OCH | OCH-1-<slot_index>-L<line_index> | SopChangeRate |  vendor extensions |   |
| OCH | OCH-1-<slot_index>-L<line_index> | SopVectorS1 |  vendor extensions |   |
| OCH | OCH-1-<slot_index>-L<line_index> | SopVectorS2 |  vendor extensions |   |
| OCH | OCH-1-<slot_index>-L<line_index> | SopVectorS3 |  vendor extensions |   |
| OCH | OCH-1-<slot_index>-L<line_index> | TxLaserAge |  vendor extensions |   |

#### OTN
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| OTN | CH<channel_index> | Esnr |  openconfig-terminal-device/terminal-device/logical-channels/channel/otn/state/esnr |   |
| OTN | CH<channel_index> | PostFecBer | openconfig-terminal-device/terminal-device/logical-channels/channel/otn/state/post-fec-ber  |   |
| OTN | CH<channel_index> | PreFecBer |  openconfig-terminal-device/terminal-device/logical-channels/channel/otn/state/pre-fec-ber |   |
| OTN | CH<channel_index> | QMargin |  vendor extensions |   |
| OTN | CH<channel_index> | QValue |  openconfig-terminal-device/terminal-device/logical-channels/channel/otn/state/q-value |   |
| OTN | CH<channel_index> | Delay |  vendor extensions |   |


#### Transceiver

| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index> | CaseTemperature | vendor extensions  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index> | InputPower | openconfig-platform/components/component/transceiver/state/input-power  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index> | LaserBiasCurrent | openconfig-platform/components/component/transceiver/state/LaserBiasCurrent  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index> | LaserBiasVoltage | vendor extensions  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index> | OutputPower | openconfig-platform/components/component/transceiver/state/output-power  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index> | PreFecBer | openconfig-platform/components/component/transceiver/state/pre-fec-ber  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index> | Temperature |  openconfig-platform/components/component/state/temperature |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index>:CH-<physical_channel_index> | InputPower | openconfig-platform/components/component/transceiver/physical-channels/state/input-power  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index>:CH-<physical_channel_index> | LaserBiasCurrent |  openconfig-platform/components/component/transceiver/physical-channels/state/laser-bias-current |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-C<client_index>:CH-<physical_channel_index> | OutputPower | openconfig-platform/components/component/transceiver/physical-channels/state/output-power  |   |

| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | CaseTemperature |  vendor extensions |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | EdfaBiasCurrent |  vendor extensions |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | InputPower | openconfig-platform/components/component/transceiver/state/input-power  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | LaserBiasCurrent | openconfig-platform/components/component/transceiver/state/LaserBiasCurrent  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | LaserBiasVoltage |  vendor extensions |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | OutputPower | openconfig-platform/components/component/transceiver/state/output-power  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | PostFecBer |  openconfig-platform/components/component/transceiver/state/post-fec-ber |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | PowerConsumption | vendor extensions  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | PreFecBer |  openconfig-platform/components/component/transceiver/state/pre-fec-ber |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | Temperature | openconfig-platform/components/component/state/temperature  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | TxModBiasXi | openconfig-platform/components/component/oc-opt-term:optical-channel/state/modulator-bias-xi  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | TxModBiasXph | openconfig-platform/components/component/oc-opt-term:optical-channel/state/modulator-bias-x-phase  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | TxModBiasXq |  openconfig-platform/components/component/oc-opt-term:optical-channel/state/modulator-bias-xq |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | TxModBiasYi | openconfig-platform/components/component/oc-opt-term:optical-channel/state/modulator-bias-yi  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | TxModBiasYph | openconfig-platform/components/component/oc-opt-term:optical-channel/state/modulator-bias-y-phase  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-L<line_index> | TxModBiasYq | openconfig-platform/components/component/oc-opt-term:optical-channel/state/modulator-bias-yq  |   |

| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-<component_index> | InputPower |  openconfig-platform/components/component/transceiver/state/input-power |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-<component_index> | OutputPower | openconfig-platform/components/component/transceiver/state/output-power  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-<component_index> | Temperature | openconfig-platform/components/component/state/temperature  |   |
| TRANSCEIVER | TRANSCEIVER-1-<slot_index>-<component_index> | LaserBiasCurrent | openconfig-platform/components/component/transceiver/state/LaserBiasCurrent  |   |

#### Panel port
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| PORT | PORT-1-<slot_index>-<panel_port_name> | InputPower | openconfig-platform/components/component/port/optical-port/state/input-power  |   |
| PORT | PORT-1-<slot_index>-<panel_port_name> | OutputPower | openconfig-platform/components/component/port/optical-port/state/output-power  |   |

#### Amplifier
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | ActualGain | openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/state/actual-gain  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | ActualGainTilt | openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/state/actual-gain-tilt  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | InputPowerTotal | openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/state/input-power-total  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | LaserBiasCurrent | vendor extensions  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | LaserTecCurrent | openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/state/laser-bias-current  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | LaserTemperature | vendor extensions  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | OutputPowerTotal | openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/state/output-power-total  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | PanelOutputPower | vendor extensions  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | Temperature | openconfig-platform/components/component/state/temperature  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | OpticalReturnLoss | openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/state/optical-return-loss  |   |
| AMPLIFIER | AMPLIFIER-1-<slot_index>-<component_index> | OutputReflectPower | vendor extensions  |   |

#### Internal Amplifier port
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| PORT | PORT-1-<slot_index>-<edfa_index>-EDFAIN | InputPower | openconfig-platform/components/component/port/optical-port/state/input-power  |   |
| PORT | PORT-1-<slot_index>-<edfa_index>-EDFAOUT | OutputPower | openconfig-platform/components/component/port/optical-port/state/input-power  |   |

#### APS Port
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| APS_PORT | APS-1-<slot_index>-<component_index>-CommonIn | OpticalPower | openconfig-transport-line-protection/aps/aps-modules/aps-module/ports/common-in/state/optical-power  |   |
| APS_PORT | APS-1-<slot_index>-<component_index>-CommonOutput | OpticalPower |  openconfig-transport-line-protection/aps/aps-modules/aps-module/ports/common-output/state/optical-power |   |
| APS_PORT | APS-1-<slot_index>-<component_index>-LinePrimaryIn | OpticalPower | openconfig-transport-line-protection/aps/aps-modules/aps-module/ports/line-primary-in/state/optical-power  |   |
| APS_PORT | APS-1-<slot_index>-<component_index>-LinePrimaryOut | OpticalPower | openconfig-transport-line-protection/aps/aps-modules/aps-module/ports/line-primary-out/state/optical-power   |   |
| APS_PORT | APS-1-<slot_index>-<component_index>-LinePrimaryOut | Attenuation |  openconfig-transport-line-protection/aps/aps-modules/aps-module/ports/line-primary-out/state/attenuation |   |
| APS_PORT | APS-1-<slot_index>-<component_index>-LineSecondaryIn | OpticalPower | openconfig-transport-line-protection/aps/aps-modules/aps-module/ports/line-secondary-in/state/optical-power  |   |
| APS_PORT | APS-1-<slot_index>-<component_index>-LineSecondaryOut | OpticalPower | openconfig-transport-line-protection/aps/aps-modules/aps-module/ports/line-secondary-out/state/optical-power  |   |
| APS_PORT | APS-1-<slot_index>-<component_index>-LineSecondaryOut | Attenuation | openconfig-transport-line-protection/aps/aps-modules/aps-module/ports/line-secondary-out/state/attenuation  |   |

#### ATTENUATOR
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| ATTENUATOR | ATTENUATOR-1-<slot_index>-<component_index> | ActualAttenuation | openconfig-optical-attenuator/optical-attenuator/attenuator/state/actual-attenuation  |   |

#### OSC
| table_name | object_index | PM_item_name | Openconfig path | description |
| :------| :---------- | :-------- | :-------- | :-------- |
| OSC | OSC-1-<slot_index>-<component_index> | InputPower |  openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/supervisory-channels/supervisory-channel/state/input-power  |   |
| OSC | OSC-1-<slot_index>-<component_index> | OutputPower | openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/supervisory-channels/supervisory-channel/state/output-power  |   |
| OSC | OSC-1-<slot_index>-<component_index> | LaserBiasCurrent |  openconfig-optical-amplifier/optical-amplifier/amplifiers/amplifier/supervisory-channels/supervisory-channel/state/laser-bias-current |   |
| OSC | OSC-1-<slot_index>-<component_index> | Temperature | openconfig-platform/components/component/state/temperature  |   |
| OSC | OSC-1-<slot_index>-<component_index> | InputOffsetLineRx | vendor extensions  |   |
| OSC | OSC-1-<slot_index>-<component_index> | OutputOffsetLineTx |  vendor extensions |   |
| OSC | OSC-1-<slot_index>-<component_index> | PanelInputPowerLineRx | vendor extensions  |   |
| OSC | OSC-1-<slot_index>-<component_index> | PanelOutputPowerLineTx | vendor extensions  |   |

### Counters statistic table 

#### Ethernet
Openconfig path: openconfig-terminal-device/terminal-device/logical-channels/channel/ethernet/state/*
```
key                             = ETHERNET:CH<channel_index>:<15_pm_current|24_pm_current|current> ; string
;field                          = value
interval                        = int              ; 900000000000 for 15 minutes interval,86400000000000 for 24 hours interval, 1000000000 since system start up
starttime                       = int              ; timestamp in nanoseconds relative to the Unix Epoch
validity                        = STRING           ; enum: incomplete, complete, invalid
in-block-errors                 = int
in-pcs-bip-errors               = int
in-pcs-errored-seconds          = int
in-pcs-severely-errored-seconds = int
in-pcs-unavailable-seconds      = int
out-block-errors                = int
in-oversize-frames              = int
in-undersize-frames             = int
in-jabber-frames                = int
in-fragment-frames              = int
in-crc-errors                   = int
out-crc-errors                  = int
tx-good-frame                   = int
tx-good-octets                  = int
tx-good-multicast               = int
tx-good-broadcast               = int
tx-good-64b                     = int
tx-good-65b-127b                = int
tx-good-128b-255b               = int
tx-good-256b-511b               = int
tx-good-512b-1023b              = int
tx-good-1024b-1518b             = int
tx-good-1519b-ltj               = int
tx-good-jumbo                   = int
tx-bad-frame                    = int
tx-bad-octets                   = int
tx-bad-multicast                = int
tx-bad-broadcast                = int
tx-bad-lt-64b                   = int
tx-bad-64b                      = int
tx-bad-65b-127b                 = int
tx-bad-128b-255b                = int
tx-bad-256b-511b                = int
tx-bad-512b-1023b               = int
tx-bad-1024b-1518b              = int
tx-bad-1519b-ltj                = int
tx-bad-jumbo                    = int
tx-octets                       = int
tx-frame                        = int
tx-broadcast                    = int
tx-multicast                    = int
tx-crc-align                    = int
tx-under-size                   = int
tx-over-size                    = int
tx-fragment                     = int
tx-jabber                       = int
tx-64b                          = int
tx-65b-127b                     = int
tx-128b-255b                    = int
tx-256b-511b                    = int
tx-512b-1023b                   = int
tx-1024b-1518b                  = int
tx-1519b-max                    = int
rx-good-frame                   = int
rx-good-octets                  = int
rx-good-multicast               = int
rx-good-broadcast               = int
rx-good-64b                     = int
rx-good-65b-127b                = int
rx-good-128b-255b               = int
rx-good-256b-511b               = int
rx-good-512b-1023b              = int
rx-good-1024b-1518b             = int
rx-good-1519b-ltj               = int
rx-good-jumbo                   = int
rx-bad-frame                    = int
rx-bad-octets                   = int
rx-bad-multicast                = int
rx-bad-broadcast                = int
rx-bad-lt-64b                   = int
rx-bad-64b                      = int
rx-bad-65b-127b                 = int
rx-bad-128b-255b                = int
rx-bad-256b-511b                = int
rx-bad-512b-1023b               = int
rx-bad-1024b-1518b              = int
rx-bad-1519b-ltj                = int
rx-bad-jumbo                    = int
rx-octets                       = int
rx-frame                        = int
rx-broadcast                    = int
rx-multicast                    = int
rx-crc-align                    = int
rx-64b                          = int
rx-65b-127b                     = int
rx-128b-255b                    = int
rx-256b-511b                    = int
rx-512b-1023b                   = int
rx-1024b-1518b                  = int
rx-1519b-max                    = int
```


#### OTN
Openconfig Path: openconfig-terminal-device/terminal-device/logical-channels/channel/otn/state/*
```
key                             = OTN:CH<channel_index>:<15_pm_current|24_pm_current|current> ; string
;field                          = value
interval                        = int              ; 900000000000 for 15 minutes interval,86400000000000 for 24 hours interval, 1000000000 since system start up
starttime                       = int              ; timestamp in nanoseconds relative to the Unix Epoch
validity                        = STRING           ; enum: incomplete, complete, invalid
errored-seconds                 = int
severely-errored-seconds        = int
unavailable-seconds             = int
fec-uncorrectable-blocks        = int
fec-corrected-bits              = int
background-block-errors         = int
sm-bei                          = int
code-violations                 = int
```

#### Transceiver
Openconfig Path: openconfig-platform/components/component/transceiver/state
```
key                             = TRANSCEIVER:TRANSCEIVER-1-<slot_index>-C<client_index>:<15_pm_current|24_pm_current|current> ; string
;field                          = value
interval                        = int              ; 900000000000 for 15 minutes interval,86400000000000 for 24 hours interval, 1000000000 since system start up
starttime                       = int              ; timestamp in nanoseconds relative to the Unix Epoch
validity                        = STRING           ; enum: incomplete, complete, invalid
loss-time                       = int              ; vendor extensions
fec-uncorrectable-words         = int
```
```
key                             = TRANSCEIVER:TRANSCEIVER-1-<slot_index>-L<line_index>:<15_pm_current|24_pm_current|current> ; string
;field                          = value
interval                        = int              ; 900000000000 for 15 minutes interval,86400000000000 for 24 hours interval, 1000000000 since system start up
starttime                       = int              ; timestamp in nanoseconds relative to the Unix Epoch
validity                        = STRING           ; enum: incomplete, complete, invalid
loss-time                       = int              ; vendor extensions
fec-uncorrectable-words         = int
```

#### Interface
Openconfig Path: openconfig-interfaces/interfaces/interface/state/counters
```
key                             = INTERFACE:INTERFACE-1-<slot_index>-<component_index>-OSC:<15_pm_current|24_pm_current|current> ; string
;field                          = value
interval                        = int              ; 900000000000 for 15 minutes interval,86400000000000 for 24 hours interval, 1000000000 since system start up
starttime                       = int              ; timestamp in nanoseconds relative to the Unix Epoch
validity                        = STRING           ; enum: incomplete, complete, invalid
in-pkts                         = int
in-errors                       = int
in-broadcast-pkts               = int
in-discards                     = int
in-fcs-errors                   = int
in-multicast-pkts               = int
in-octets                       = int
in-unicast-pkts                 = int
in-unknown-protos               = int
out-broadcast-pkts              = int
out-discards                    = int
out-multicast-pkts              = int
out-octets                      = int
out-pkts                        = int
out-unicast-pkts                = int
```

# HISTORY_DB, index 10 (share logical instance with GB_COUNTERS_DB)
The OTN historical PM data (gauge and counters statistics) and alarms are stored in HISTORY_DB.

### Historical gauge statistic table template
The `table_name`, `object_index` and `PM_item_name` is the same as the current PM tables in COUNTER_DB, but the history PM table contains the PM bin start timestamp. 
```
key                       = <table_name>:<object_index>_<PM_item_name>:<15_pm_history|24_pm_history>_<starttime> ; string  
;field                    = value  
interval                  = int              ; 900000000000 for 15 minutes interval,86400000000000 for 24 hours interval  
starttime                 = int              ; timestamp in nanoseconds relative to the Unix Epoch  
max                       = int | float64    ; float64 with fraction-digits 1, 2, and 18   
max-time                  = int              ; timestamp in nanoseconds relative to the Unix Epoch  
min                       = int | float64    ; float64 with fraction-digits 1, 2, and 18  
min-time                  = int              ; timestamp in nanoseconds relative to the Unix Epoch  
instant                   = int | float64    ; float64 with fraction-digits 1, 2, and 18  
avg                       = int | float64    ; float64 with fraction-digits 1, 2, and 18  
validity                  = STRING           ; enum: incomplete, complete, invalid  
```
### Counters statistic table 
The OTN historical counters statistics contains the PM start timestamp, and the fields are the same as current PM.

#### Ethernet
```
key                             = ETHERNET:CH<channel_index>:<15_pm_history|24_pm_history>_<starttime> ; string
```
#### OTN
```
key                             = OTN:CH<channel_index>:<15_pm_history|24_pm_history>_<starttime> ; string
```
#### Transceiver
```
key                             = TRANSCEIVER:TRANSCEIVER-1-<slot_index>-C<client_index>:<15_pm_history|24_pm_history>_<starttime> ; string
key                             = TRANSCEIVER:TRANSCEIVER-1-<slot_index>-L<line_index>:<15_pm_history|24_pm_history>_<starttime> ; string
```
#### Interface
```
key                             = INTERFACE:INTERFACE-1-<slot_index>-<component_index>-OSC:<15_pm_history|24_pm_history>_<starttime> ; string
```

### Historical alarm table template
```
key                       = HISALARM:<alarm_id>_<time-cleared> ; string  
;field                    = value  
id                        = STRING           ; the unique id  
time-created              = int              ; timestamp in nanoseconds relative to the Unix Epoch  
resource                  = STRING           ; the OTAI object name,  it may be a component, ports,channel, etc 
text                      = STRING           ; the alarm information text 
severity                  = STRING           ; enums MINOR, WARNING, MAJOR, CRITICAL
type-id                   = STRING           ; enums defined in OTAI alarm list  
time-cleared              = int              ; timestamp in nanoseconds relative to the Unix Epoch  
```
