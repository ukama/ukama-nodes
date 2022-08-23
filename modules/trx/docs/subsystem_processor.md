#### Processor

LTE-SoC processor used in TRX module is CNF7130 from Marvell. It is MIPS64 based Quad core System-on-chip with below features: 
* Core frequency up to 1.3GHz and Co-processor frequency up to 800MHz
* High-bandwidth fully coherent on-chip memory system including a 1MB 16-way set-associative L2 cache
* A 72-bit DDR3 DRAM interface, up to 1066 MHz data rate
* Integrated Baseband processing module containing three radio-processing clusters: 
    * An uplink symbol radio-processing cluster, 
    * An uplink soft-bit radio processing cluster, 
    * A downlink radio processing cluster

Below diagram provides high level information about internal modules of LTE-SoC:

![processor](https://ukama-site-assets.s3.amazonaws.com/hardware/Processor.png)


The Local Memory Controller (LMC) DDR3 SDRAM controller conforms to the JEDEC DDR3 SDRAM specification. 
Up to 32GB DDR3 RAMs can be connected to LMC SDRAM controller. In TRX module, 4GB DDR3 RAM is used with Data rates up to 1066 MHz.

The Level-2 Cache Controller (L2C) has shared on-chip cache memory which is shared be all of the cores and I/O components on LTE-SOC. 

LTE-SoC also implements a boot-bus interface that can be interfaced with NOR, eMMC, or SD flash devices. In TRX module, NOR flash is used as primary boot device and eMMC memory is used as additional storage device. 
There are 8 Chip selects and 32bit Address/Data lines in boot bus.
When configured in 8bit data width option, BOOT_AD[31:24] is the data bus.
The boot bus consumes a 4GB portion of the LTE-SoC physical-address (PA) space range from 0x1 0000 0000 0000 to 0x1 0000 FFFF FFFF. 
In TRX module, Chip select 0 (CE_0) is connected to NOR Flash for primary booting purpose. 
LTE-SoC maps PA space to 0x1 0000 1FC0 0000 for CE_0.
Boot code should be present in the NOR flash starting at boot address 0. When the LTE-SoC self-boots, core 0 executes this code immediately following chip-reset de-assertion. 
The pin straps that LTE-SoC uses for configuration are shown below:
| Pin Strap     | Comment                                                                                                                                                                   |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BOOT_AD[15]   | An external pull-up resistor changes region 0 from nonmultiplexed (i.e. nonALE mode) to multiplexed (i.e. ALE) mode.                                                      |
| BOOT_AD[14]   | An external pull-up resistor changes region 0 from eight-bit data width to 16-bit data width                                                                              |
| BOOT_AD[12]   | An external pull-up resistor changes the default polarity of BOOT_DMACK1 from active-high to active-low                                                                   |
| BOOT_AD[11]   | An external pull-up resistor changes the default polarity of BOOT_DMACK0 from active-high to active-low                                                                   |
| BOOT_AD[10:9] | External pull-up resistors that change the boot-bus-driver drive strength in the following manner: <br /> [00] : Full strength <br /> [01] : 20 ohm <br /> [10] : 50 ohm <br /> [11] : 60 ohm |
| BOOT_AD[8]    | When an external pull-down resistor is connected, SoC will boot from NOR Flash                                                                                            |
| BOOT_AD[7:0]  | When using eMMC, BOOT_AD[7:0] is EMMC_DAT[7:0].                                                                                                                           |


