####Architecture overview

TRX module is based on a LTE-SoC along with RF Transceiver. 
Below diagram represents the high level architecture overview of TRX module: 

![high level architecture diagram](https://ukama-site-assets.s3.amazonaws.com/hardware/High%20level%20diagram2.png) 

A detailed representation of TRX module subsystems along with their interfaces is as shown below:

![detailed architecture diagram](https://ukama-site-assets.s3.amazonaws.com/hardware/TRX_Block_diag_07Aug.png)

CNF7130 from Marvell is used as LTE-SoC in TRX module. It will have 4GB DDR3 memory as RAM connected via DDR3 bus of LTE-SoC, 128MB NOR Flash and 32GB eMMC memories connected to Boot bus of LTE-SOC acts as storage devices. 
AD9363 from Analog is used as RF Transceiver which is connected to LTE-SoC via two ports of 12bit data bus along with control signals. 
An Ethernet switch from Marvell 88E6341, acts as bridge between LTE-SoC, COM Module and External RJ45 connector. 
A GPS module and an OCXO along with DAC are used to cater for timing requirements of TRX module. 
 