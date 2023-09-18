

# How to Build AONOS Virtual Machine


1. Clone sonic-buildimage repo
```
git clone git@github.com:zhengweitang-zwt/sonic-buildimage.git
```
2. Clone sub-repos
```
make init
```

3. Config platform type

```
make configure PLATFORM=vs
```

4. Build kvm image

```
make target/sonic-vs.img.gz
```

5. Get vm binaries

```
ONIE image: onie-recovery-x86_64-kvm_x86_64_4_asic-r0.iso
AONOS image: sonic-4asic-vs.img, sonic-installer.img
```



# How to Start a Virtual Machine

1. Install qemu
```
sudo apt-get install qemu-kvm
```

2. Start VM via qemu
```
sudo qemu-system-x86_64 -m 3072 -name onie -accel hvf -boot order=cd,once=d -cdrom ./onie-recovery-x86_64-kvm_x86_64_4_asic-r0.iso  -device e1000,netdev=onienet -netdev user,id=onienet,hostfwd=:0.0.0.0:3040-:22 -vnc 0.0.0.0:0 -vga std -drive file=./sonic-4asic-vs.img,media=disk,if=virtio,index=0 -drive file=./sonic-installer.img,if=virtio,index=1 -serial telnet:127.0.0.1:7000,server
```

3. Open a new terminal, use console to login ONIE.
```
telnet 127.0.0.1 7000
```

4. Choose entry: `ONIE: Embed ONIE`
<img src="ONIE snapshot 1.jpg" alt="ONIE snapshot 1" style="zoom: 50%;" />

5. Choose entry: `ONIE: Install OS`
<img src="ONIE snapshot 2.jpg" alt="ONIE snapshot 2" style="zoom: 50%;" />

6. Wait until AONOS image installed.
  
   Use 'blkid' to check if SONiC-OS partition is created.

```
ONIE:/ # blkid
/dev/vda3: LABEL="SONiC-OS" UUID="4e05cdd8-b229-4dbe-b0e9-c46f463f6222"
/dev/vdb: UUID="9DBD-175E"
/dev/vda2: LABEL="ONIE-BOOT" UUID="e0f5d6f2-64aa-4501-8123-62bed94e04ee"
```


7. reboot
```
ONIE:/ # reboot
```

8. Boot from SONiC-OS partition.
<img src="ONIE snapshot 3.jpg" alt="ONIE snapshot 3" style="zoom: 50%;" />


# How to Login Virtual Machine

1. Connect AONOS VM via console

```
telnet 127.0.0.1 7000
```

2. Connect AONOS VM via SSH

    a. Connect via console

    b. Request a new DHCP address

    ```
    sudo dhclient -v
    ```

    c. Connect via SSH
    ```
    ssh root@localhost -p 3040
    ```

# How does Virtual Machine simulate line-card and driver
<img src="AONOS VM.png" alt="AONOS VM" style="zoom: 50%;" />




# AONOS CLI Demo

1. Show line-cards info
```
root@sonic:~# show chassis 1 slots
Succeeded
  Slot  Prov    Status    PN            SN            Hard-Ver    Soft-Ver    Temp(C)
------  ------  --------  ------------  ------------  ----------  ----------  ---------
     1  P230C   Ready     P230CAC000    AC049015C007  1.1         1.1.1       24.0
     2  P230C   Ready     P230CAC000    AC049015C008  1.1         1.1.1       27.0
     3  P230C   Ready     P230CAC000    AC049015C009  1.1         1.1.1       29.0
     4  E110C   Ready     E110CAC000    AC04B1167003  1.1         1.1.1       29.0
     5  PSU     Ready     GW-CRPS800N2  2K120052945   2.0         N/A         21.1
     6  PSU     Empty     N/A           N/A           N/A         N/A         NA
     7  FAN     Ready     FANAC000      AC042315901A  1.1         N/A         51.1
     8  FAN     Ready     FANAC000      AC042315901B  1.1         N/A         52.2
     9  FAN     Ready     FANAC000      AC042315901C  1.1         N/A         53.3
    10  FAN     Ready     FANAC000      AC042315901D  1.1         N/A         54.4
    11  FAN     Ready     FANAC000      AC042315901E  1.1         N/A         55.5
root@sonic:~# 
```

2. Show client-side transceiver info
```
root@sonic:~# show slot 1 client 1 state
Succeeded
Name                                         : TRANSCEIVER-1-1-C1
Exist                                        : PRESENT
PortAdmin                                    : ENABLED
TxLaser                                      : true
PowerMode                                    : NORMAL
PrecfgType                                   : ETH_10GBASE_LRM
FecMode                                      : FEC_AUTO
LoopBack                                     : NONE
prbs                                         : true
prbsPattern                                  : PRBS_PATTERN_TYPE_2E7
LinkDownDelayMode                            : true
LinkDownDelayHoldoff(ms)                     : 0
LinkUpDelayMode                              : true
LinkUpDelayHoldoff(s)                        : 0
LinkUpDelayActiveThreshold(ms)               : 0
DelayMeasAutoEnable                          : true
ClientAlsClientRxSwitch                      : NONE
ClientAlsClientRxHoldoff(ms)                 : 0
ClientAlsClientTxSwitch                      : NONE
ClientAlsClientTxHoldoff(ms)                 : 0
ClientAlsLineRxSwitch                        : NONE
ClientAlsLineRxHoldoff(ms)                   : 0
ForceInsert                                  : LF
LldpEnable                                   : true
ExtendModuleCode                             : SDR
Datastatus                                   : NOT_READY
Encode                                       : UNSPECIFIED
Bitrate(Gbit)                                : 0.0
Extendbitrate(Gbit)                          : 0.0
GeLinkState                                  : UP
ClientMode                                   : PROT_1GE
LedState                                     : RED

RxPowerTotal(dBm)                            : 75.0
TxPowerTotal(dBm)                            : 75.0
Temp(C)                                      : 75.0
Voltage(mV)                                  : 75.0
PreFecBer                                    : 75.0
DelayMeasureValue(us)                        : 75

<Lane-1>
InputPower(dBm)                              : 74.0
OutputPower(dBm)                             : 74.0
LaserBias(mA)                                : 74.0
<Lane-2>
InputPower(dBm)                              : 74.0
OutputPower(dBm)                             : 74.0
LaserBias(mA)                                : 74.0
<Lane-3>
InputPower(dBm)                              : 74.0
OutputPower(dBm)                             : 74.0
LaserBias(mA)                                : 74.0
<Lane-4>
InputPower(dBm)                              : 74.0
OutputPower(dBm)                             : 74.0
LaserBias(mA)                                : 74.0

root@sonic:~# 
```


3. Show line-side transceiver info
```
root@sonic:~# show slot 1 line 1 state
Succeeded
Name                                         : TRANSCEIVER-1-1-L1
PortOperState                                : ACTIVE
PortAdministate                              : ENABLED
DcoVendorPreconf                             : test data
tx-laser                                     : true
LineLoop                                     : NONE
LinePrbsTx                                   : PRBS_PATTERN_TYPE_2E7
LinePrbsRx                                   : PRBS_PATTERN_TYPE_2E7
LineForceInsert                              : AIS
tti-msg-auto-mode                            : true
Tti_transmit                                 : test data
Tti_rx_expected                              : test data
TxPower(dBm)                                 : 0.0
Frequency(MHz)                               : 0
OperationlMode                               : test data
Tti_rx                                       : test data
LineState                                    : true
LineExist                                    : PRESENT
LineFrequency(MHz)                           : 0
LinePrbsStateTx                              : PRBS_PATTERN_TYPE_2E7
LinePrbsStateRx                              : PRBS_PATTERN_TYPE_2E7
LedState                                     : RED

InputPower(dBm)                              : 2907.0
OutputPower(dBm)                             : 2907.0
LaserBias(mA)                                : 2907.0
PowerConsumption(W)                          : 2907.0
CaseTemp(C)                                  : 2907.0
LaserTemp(C)                                 : 2907.0
TX MOD BIAS X/I                              : 2907.0
TX MOD BIAS X/Q                              : 2907.0
TX MOD BIAS Y/I                              : 2907.0
TX MOD BIAS Y/Q                              : 2907.0
TX MOD BIAS X/PH                             : 2907.0
TX MOD BIAS Y/PH                             : 2907.0
Q-Factor                                     : 2907.0
ESNR                                         : 2907.0
PRE-FEC-BER                                  : 2907.0
POST-FEC-BER                                 : 2907.0
OSNR(dB)                                     : 2907.0
CD(ps/nm)                                    : 2907.0
PDL(dB)                                      : 2907.0
FOFF(MHz)                                    : 312758597
DGD(ps)                                      : 2907.0
SoPMD(ps^2)                                  : 2907.0
SOP CHANGE RATE(rad/s)                       : 312758597
LASER BIAS(mA)                               : 2907.0
EDFA BIAS(mA)                                : 2907.0
Signal Input Power(dBm)                      : 2907.0

root@sonic:~# 

```

4. Show edfa module info

```
root@sonic:~# show slot 4 edfa 1 info
Succeeded
Module Name                                  : AMPLIFIER-1-4-1
Module PN                                    : test data
Module SN                                    : test data
Firmware version                             : test data
Work mode                                    : CONSTANT_POWER
EDFA Switch Actual                           : true
Gain Range(dB)                               : LOW_GAIN_RANGE
Set gain(dB)                                 : 0.0
Set Gain tilt                                : 0.0
Work State                                   : LOS_A
Los Ase Delay(hold off time)(ms)             : 0
Input LOS Th(dBm)                            : 0.0
Input LOS Hy(dB)                             : 0.0
Output LOP Th(dBm)                           : 0.0
Output LOP Hy(dB)                            : 0.0
Gain Low Alm Th(dBm)                         : 0.0
Gain Low Alm Hy(dB)                          : 0.0
Pin Low AlmTh(dBm)                           : 0.0
Pout Low AlmTh(dBm)                          : 0.0

Module temperature(C)                        : 3035.0
Actual gain(dB)                              : 3035.0
Actual Gain tilt                             : 3035.0
Pump Iop(mA)                                 : 3035.0
Pump temperature(C)                          : 3035.0
Pump Itec(mA)                                : 3035.0
Input power(Original)(dBm)                   : 3035.0
Output power(Original)(dBm)                  : 3035.0
```

5. Show olp info

```
root@sonic:~# show slot 4 olp 1 info
Succeeded
Module Name                                  : APS-1-4-1
Work Mode (Revertive)                        : true
Work Line                                    : PRIMARY
Wait-to-restore-time(ms)                     : 0
Hold-off-time(ms)                            : 0
Switch Hysteresis(dB)                        : 0.0
Alarm Hysteresis(dB)                         : 0.0
Relative Diff Threshold(dB)                  : 0.0
Relative Diff Threshold Offset(dB)           : 0.0
Force-to-port                                : NONE
primary-in Switch Th.(dBm)                   : 0.0
secondary-in Switch Th.(dBm)                 : 0.0

LinePrimaryIn Los Th.(dBm)                   : 0.0
LinePrimaryIn Low Th.(dBm)                   : 0.0
LinePrimaryIn Optical Power(dBm)             : 3066.0
LinePrimaryOut Los Th.(dBm)                  : 0.0
LinePrimaryOut Low Th.(dBm)                  : 0.0
LinePrimaryOut Optical Power(dBm)            : 3066.0
LineSecondaryIn Los Th.(dBm)                 : 0.0
LineSecondaryIn Low Th.(dBm)                 : 0.0
LineSecondaryIn Optical Power(dBm)           : 3066.0
LineSecondaryOut Los Th.(dBm)                : 0.0
LineSecondaryOut Low Th.(dBm)                : 0.0
LineSecondaryOut Optical Power(dBm)          : 3066.0
CommonIn Los Th.(dBm)                        : 0.0
CommonIn Low Th.(dBm)                        : 0.0
CommonIn Optical Power(dBm)                  : 3066.0
CommonOutput Los Th.(dBm)                    : 0.0
CommonOutput Low Th.(dBm)                    : 0.0
CommonOutput Optical Power(dBm)              : 3066.0

root@sonic:~# 
```