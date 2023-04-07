The primary communication medium between TRX module, COM module and backhaul is through Ethernet.
A multi-port Ethernet switch 88E6341 from Marvell is used in TRX module which will act as a bridge. A high level implementation of Ethernet subsystem is as shown below: 

![Ethernet block](https://ukama-site-assets.s3.amazonaws.com/hardware/Ethernet%20block.png)

The 6-port ethernet switch has dedicated configurations for each port. The devices contain four 10/100/1000 triple speed Ethernet transceivers (PHYs), one SERDES interface 
and one digital interface that supports a combination of RGMII, MII, and RMII interfaces. SERDES interface supports 2.5 Gbps, 1000BASE-X, and SGMII. 

Port-5 is a SERDES interface that can be configured in one of the following modes:
* SGMII mode for connection to a triple speed PHY
* 2.5 Gbps SERDES mode for connections to a CPU or another switch that supports 2.5 Gbps SERDES operation
* 1000BASE-X mode

In TRX module, the main communication interface between LTE-SoC and Ethernet switch is 1000BASE-X. The Ethernet switch can be configured in 1000BASE-X mode by pulling up P5_SMODE pin to 3.3V. 
To configure SERDES port of LTE-SOC in 1000BASE-X mode, Auto-negotiation is disabled by writing into corresponding SERDES control register. The Max data rate of SERDES port when configured in 1000BASE-X will be 1Gbps.

Ethernet switch has multiple boot options. It can be operated in stand alone mode or it can be configured in CPU mode, where in it will be managed by an external CPU. 
By default, ethernet switch will be configured in standalone mode, by driving NO_CPU pin high during reset release. This pin selects if the device is to be managed by an external CPU or stand alone mode. In TRX module, provision has been made to operate ethernet switch in CPU mode by connecting MDC and MDIO signals to SMI port of LTE-SoC. 
An optional 2-wire EEPROM is also provided in TRX module to support EEPROM boot. Additionally it can also be configured in boot via SMI interface. 


For synchronous Ethernet requirement, the switch has a provision to accept SyncE clock source. Each PHY, via a PHY register, can select this clock input as its reference clock input instead of using the default XTAL_IN input. In case of TRX module, this pin is connected to a GPIO of LTE-SoC. Whenever there is a need for SynCE, LTE-SoC can drive this pin with reference to its internal clock. 

1000Base-X mode uses a modified LVDS specification based on the IEEE standard 1596.3. 



