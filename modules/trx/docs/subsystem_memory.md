###Memory Sub-system

####DDR RAM

The LTE-SoC has an internal LMC DDR3 SDRAM controller which conforms to the JEDEC DDR3 SDRAM specification. A total of 4 DDR3 ICs, with 512MB density each are connected to provide a memory capacity of 4GB. One additional DDR3 IC is provided for SECDED ECC purpose. 

DDR3 memory architecture is as shown below: 

![DDR](https://ukama-site-assets.s3.amazonaws.com/hardware/DDR.png)

DDR subsystem operates at a voltage of 1.35V. 72bit (64bit data + 8bit ECC) bus is configured to work with data transfer rate at 1066MHz.

Below table lists out different DDR signals and their functionality:

|     Signal   Name                                     |     Direction  |      Description                                                                                 |
|-------------------------------------------------------|----------------|--------------------------------------------------------------------------------------------------|
| **_    Rank 0 –   Data Signal Group   _**             |                |                                                                                                  |
|     DDR_DQ[63:0]                                      |     In/Out     |     Data Bus                                                                                     |
|     DDR_DQSP[7:0], DRAM0_DQSN[7:0]                    |     In/Out     |     Data Strobes                                                                                 |
|     DDR_CB[7:0]                                       |     In/Out     |     ECC Data Bits                                                                                |
|     DDR_CBS0P, DDR_CBS0N                              |     In/Out     |     ECC Data Strobes                                                                             |
| **_     Rank 0 – Command Signal Group   _**           |                |                                                                                                  |
|     DDR_A[15:0]                                       |     Out        |     Memory Address Bus                                                                           |
|     DDR_BA[2:0]                                       |     Out        |     Bank Select                                                                                  |
|     DDR_RAS#                                          |     Out        |     Row Address Select                                                                           |
|     DDR_CAS#                                          |     Out        |     Column Address Select                                                                        |
|     DDR_WE#                                           |     Out        |     Write Enable                                                                                 |
| **_    Rank 0 –   Control Signal Group   _**          |                |                                                                                                  |
|     DDR_DIMM0_CS0_L                                   |     Out        |     Chip Select (one per rank)                                                                   |
|     DDR_DIMM0_CKE0                                    |     Out        |     Clock Enable (one per rank).                                                                 |
|     DDR_DIMM0_ODT0                                    |     Out        |     On-Die Termination Select/Enable                                                             |
| **_    Rank 0 –   Clock Signal Group   _**            |                |                                                                                                  |
|     DDR_CK0P                                          |     Out        |     Differential Clocks                                                                          |
|     DDR_CK0N                                          |     Out        |     Inverted Differential Clocks                                                                 |
| **_    DDR3L Reference and Compensation Signals   _** |                |                                                                                                  |
|     DDR_VREF                                          |     In         |     SDRAM Reference Voltage: external    reference voltage input for each system    margining    |
|     DDR_PLL_VDD33                                     |     In         |     DDR3L Power Good Monitor.                                                                    |
|     DDR_RESET_L                                       |     Out        |     DDR3L DRAM Reset.                                                                            |
|     DDR_COMP_DP    DDR_COMP_DN                        |     In/Out     |     DDR3L Compensation signals                                                                   |


####NOR Flash

TRX module is equipped with a 128MB parallel NOR flash which is configured as primary boot device. 

Below diagram illustrates the NOR flash connection with LTE-SoC:

![NOR](https://ukama-site-assets.s3.amazonaws.com/hardware/NOR.png)