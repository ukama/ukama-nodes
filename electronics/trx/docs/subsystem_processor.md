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

Shared main memory (implemented via the level-2 cache and the DRAM) is the primary communication media for bulk transfers in the LTE-SoC. 

Level-2 cache is shared by all of the CPU cores instructions/data and hardware device DMA accesses. The L2 cache can be partitioned, and can be bypassed on a reference-by-reference basis. I/O devices can DMA data directly into L2 cache.



##### LTE-SoC Internal Clock Architecture

LTE-SoC has a single reference clock signal (PLL_REF_CLK) that feeds into three PLLs (Core PLL, Co-processor PLL and DDR PLL) that provide the clock signals for the rest of the chip.
Each PLL has its own set of PLL_REF_CLK multipliers, allowing each to generate its own frequency.

The Core PLL generates the core clock, which is used by the MIPS cores. The frequency is configured by using pin straps during Reset release. PLL_MUL_[4:0] pins are used as below: 

| PLL_MUL[4:0] | Clock Frequency |
|--------------|-----------------|
| 0x02         | 400MHz          |
| 0x06         | 600MHz          |
| 0x0A         | 800MHz          |
| 0x0E         | 1.0GHz          |
All other options are reserved

The Coprocessor PLL generates the coprocessor clock, which is used by the rest of the coprocessors and the I/O interfaces. The frequency is determined by pin straps connected to SYS_PLL_MUL_[4:0] pins and are listed as below:

| SYS_PLL_MU[4:0] | Clock Frequency |
|-----------------|-----------------|
| 0x02            | 400MHz          |
| 0x06            | 600MHz          |
| 0x0A            | 800MHz          |
All other options are reserved

Below diagram shows the internal clock scheme of LTE-SoC:

![processor-clock](https://ukama-site-assets.s3.amazonaws.com/hardware/Processor-Clock%20internal.png)

Notes:-
      w : SMI_CLK[PHASE]  

QLM0 Serdes clock will be derived from an external source. In TRX module, a spread spectrum clock generator IC is used as source which will produce a clock of 100MHz.

Whereas, QLM1 Serdes clock can be either derived from an external source or from QLO0 reference clock. In TRX module, QLM0 reference clock is used to source QLM1 Serdes also. 

USB PLL will have a dedicated crystal connected to its  XO and XI pins.

Note that the core clock, the coprocessor clock, the DDR3 clock, and the PCIe interface have the following interdependencies:
* the core-clock frequency must be greater than or equal to the coprocessor-clock frequency
* the core-clock frequency must be greater than or equal to the DDR3-clock frequency
* PCIe Gen II requires a coprocessor-clock frequency of 600 MHz.

##### Boot process

LTE-SoC implements a boot-bus interface that can be interfaced with NOR, eMMC, or SD flash devices. In TRX module, NOR flash is used as primary boot device and eMMC memory is used as additional storage device. 
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

##### GPIO 

The GPIO bus is a collection of general-purpose pins that can be configured for various tasks, and is a low-speed bus. Each pin can be configured as input, output, clock-like output, level-sensitive/edge-triggered interrupt pins. 

In TRX module, GPIO's are used for different purposes as listed in below table : 

| GPIO # |   DIR  | Default state |    Connected to    |                                     Functionality                                     |
|:------:|:------:|:-------------:|:------------------:|:-------------------------------------------------------------------------------------:|
|    0   |   Out  |       1       |         LED        | General purpose LED                                                                   |
|    1   |   In   |       0       | Temperature Sensor | Critical temperature alert                                                            |
|    2   |   Out  |       1       |     JTAG header    | JTAG Soft reset                                                                       |
|    3   |   Out  |       0       |   Ethernet Switch  | Ethernet SYNCE                                                                        |
|    4   |   In   |       0       |     GPS module     | GPS time pulse                                                                        |
|    5   |   Out  |       0       |   Watchdog timer   | Watchdog Input                                                                        |
|    6   |   In   |       0       |     COM module     | Power good of COM module                                                              |
|    7   |   In   |       0       |     Mask module    | Interrupt from Mask module                                                            |
|    8   |   Out  |       0       |        eMMC        | Dedicated pin to enable eMMC                                                          |
|    9   |   In   |       0       |       Switch       | High indicates LTE-SoC is I2C   master<br>     Low indicates COM module is I2C master |
|   10   | In/Out |       0       |     COM module     | General purpose I/Os from COM module                                                  |
|   11   |   In   |       0       |      Connector     | External 1pps reference input                                                         |
|   12   |   Out  |       0       |     COM module     | COM module power recycle                                                              |
|   13   | In/Out |       0       |     COM module     | General purpose I/Os from COM module                                                  |
|   14   | In/Out |       0       |     COM module     | General purpose I/Os from COM module                                                  |
|   15   |   In   |       0       |     COM module     | Platform Reset information from COM module                                            |
|   16   |   In   |       0       |     I2C switch     | TRX module I2C interrupt                                                              |
|   17   |   In   |       0       |     COM module     | COM module I2C interrupt                                                              |
|   18   |   In   |       1       |    Reset switch    | Push button reset input                                                               |
|   19   |   Out  |       1       |        Reset       | System reset out from LTE-SoC                                                         |


##### Debug

LTE-SoC has multiple debug features, including:
* Two UARTs for connection to an external console.
* Full core support for the MIPS EJTAG standard, including single-step and an external JTAG interface with one internal TAP controller per core.

In TRX module, a provision is been made to connect EJTAG connector. 
Main debug port is UART0. Boot logs during system boot will be transmitted through UART0 port at 115200 baud rate. 
TX and RX pins of UART0 is connected to an isolator and then connected to USB to UART converter IC to have a mini-B USB connector as debug port.
The 5V required by the converter IC is derived from the USB connector. 

Optionally, UART1 signals are also brought to a 3 pin header. 

![processor-debug](https://ukama-site-assets.s3.amazonaws.com/hardware/Processor-debug.png)


##### SerDes

The PCIe and SGMII interfaces are shared SerDes interfaces of LTE-SoC. The SerDes interface is made up of two two-lane modules (DLM0 and DLM1). 
The DLMs have the following configurations:
* DLM0: 2× SGMII/1000 Base-X
* DLM1: PCIe (one ×2 or 2 ×1)



