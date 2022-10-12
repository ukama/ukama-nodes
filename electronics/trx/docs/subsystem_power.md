###Power overview

####Power tree

TRX module is powered with IEEE 802.3bt Type 4 Class 8 PoE. A PD controller is used along with dual ideal diode bridge controller to receive power from  data/spare pairs of ethernet cable. PD controller is configured to convert 50-57V of PoE supply to 12V DC supply which will be input supply for the TowerNode system. A provision of connecting DC supply from external power source is also made. In scenarios where both PoE and external DC supply are connected, system will disable PoE and considers external DC supply only. 

Below diagram represents the power architecture of TRX module:

![power tree](https://ukama-site-assets.s3.amazonaws.com/hardware/Power%20Tree_1.png)

Input supply,12V goes through a PMBus/I2C complaint controller to measure, protect and control the electrical operating conditions. 
Protected 12V supply is provided to COM module, mask module and is also converted to 5V from a step-down DC/DC converter. 5V is then converted to various supplies like 3.3V through a step down converter, 1.1V for LTE-SoC through a buck converter and 1.35V for LTE-SoC through a step down converter. 
3.3V is a major supply for TRX module and is used for LTE-SoC, NOR Flash, eMMC, Ethernet PHY, OCXO, GPS module, sensors and EEPROMs. Secondary supplies of LTE-SoC such as 1.5V, 1.8V and VTT supply for DDR memories are generated from 3.3V supply through voltage regulators. 1.8V is further converted to 1.3V required for RF Transceiver through a LDO. 

####Power Sequence

Power sequence of TRX module is as shown below: 

--Power sequence diagram to be inserted here -- 

High level Power sequence followed in TRX module design is in the order of System supplies -> LTE-SoC -> DDR -> System Reset release. 

System input supply 12V is converted to 5V initially with a time delay of 10ms. The step-down converter used to convert 5V to 3.3V will have a delay of 5ms. 
Once 3.3V is stable, the 3.3V power good signal asserted from step-down converter will drive enable pins of buck/dtep-down converters of LTE-SoC power supplies(1.8V, 1.5V and 1.1V). 

A stable 1.1V will assert power good signal from buck converter which is used to enable remaining power supplies of LTE-SoC (1.35V and VTT).

The 1.35V power good is then delayed by 150ms to ensure LTE-SoC and other peripherals are in powered-up state before releasing the Reset. 



####Power Dissipation

Below table lists out the maximum power dissipated by major ICs in TRX module: 

|     Sl.No.    |     Component   P/N            |     Description                                          |     Manufacturer             |     Power   (W)    |
|---------------|--------------------------------|----------------------------------------------------------|------------------------------|--------------------|
|     1         |     CNF7130-1000BG772-G        |     Cavium   OCTEON Fusion CNF7130                       |     Cavium                   |     18             |
|     2         |     IS43TR16256AL-15HBLI-ND    |     4Gbit   DDR3L SDRAM, FBGA 96                         |     ISSI                     |     0.405          |
|     3         |     IS43TR16256AL-15HBLI-ND    |     4Gbit   DDR3L SDRAM, FBGA 96                         |     ISSI                     |     0.405          |
|     4         |     IS43TR16256AL-15HBLI-ND    |     4Gbit   DDR3L SDRAM, FBGA 96                         |     ISSI                     |     0.405          |
|     5         |     IS43TR16256AL-15HBLI-ND    |     4Gbit   DDR3L SDRAM, FBGA 96                         |     ISSI                     |     0.405          |
|     6         |     IS43TR16256AL-15HBLI-ND    |     4Gbit   DDR3L SDRAM, FBGA 96                         |     ISSI                     |     0.405          |
|     7         |     88E6341                    |     Gigabit   Energy Switch                              |     Marvell                  |     0.726          |
|     8         |     MLC1260-401MLB             |     Fixed   Inductors Power Inductor 0.4 uH 20 %         |     Coilcraft                |     0.103          |
|     9         |     VLF10045T-2R2N7R4          |     INDUCTOR   POWER 2.2UH 6.3A SMD,±30%                 |     TDK                      |     0.1632         |
|     10        |     ASPI-0530HI-1R5M-T2        |     INDUCTOR   POWER 1.5UH 5A SMD +/-20%                 |     Abracon   Corporation    |     0.125          |
|     11        |     PI6C557-03BLE              |     IC   CLOCK GENERATOR 16TSSOP                         |     Pericom                  |     0.33           |
|     12        |     TPS53353DQPT               |     IC   REG BUCK SYNC ADJ 20A 22 pin QFN                |     TI                       |     0.55           |
|     13        |     TPS54620RGYR               |     Synchronous   Step-down Voltage Converter 6A         |     TI                       |     1.08           |
|     14        |     AP3418KTR-G1               |     1500mA   Step-Down Regulator Adjustable              |     Diodes   Inc             |     0.1125         |
|     15        |     TPS54519RTET               |     IC   REG BUCK SYNC ADJ 5A 16WQFN                     |     TI                       |     0.72           |
|     16        |     BGA824N6                   |     The   BGA824N6 is a front-end low noise amplifier    |     infineon                 |     0.198          |
|     17        |     AD9363ABCZ                 |     RF   Transceiver                                     |     Analog   Device          |     1.874          |
|     18        |     LTM8056EY#PBF              |     IC   MOD BUCK REG 58VIN 121BGA                       |     Linear   Technology      |     3.5            |
|     19        |     LMZ14203HTZX/NOPB          |     IC   BUCK SYNC ADJ 3A TO-PMOD-7                      |     TI                       |     1.3            |
|     20        |     TPS7A7300RGWR              |     IC   REG LDO FIX/ADJ 3A 20VQFN                       |     TI                       |     0.96           |
|     21        |     TPS7A7300RGWR              |     IC   REG LDO FIX/ADJ 3A 20VQFN                       |     TI                       |     0.32           |
|     22        |     TPS7A7300RGWR              |     IC   REG LDO FIX/ADJ 3A 20VQFN                       |     TI                       |     1.19           |


