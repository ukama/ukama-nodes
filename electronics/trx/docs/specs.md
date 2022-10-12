####Operational requirements
TRX Board along with a COM board comprises as TowerNode system. The TowerNode system is designed to meet the following requirements:

•	Powered by IEEE 802.3bt PoE (71.3 Watt Max)
•	Interface to the backhaul is over a RJ45 (Gigabit Ethernet) interface
•	Interface to the RF Front End (AmplifierNode) through cables connected over the SMA connector
•	Maximum RF output from TowerNode is -14dBm

####Environmental requirements
The TowerNode system is designed to operate at below environmental conditions

•	Ambient temperature: -20 °C to +50 °C
•	Altitude: Up to 20,000 feet
•	Solar load: Max 1100W/m2

####Hardware features
•	TowerNode is compliant with IEEE 802.3bt standard Powered Device with maximum power rating of 71.3W. 
•	TowerNode system architecture comprises of two modules: TRX and COM
•	TRX module is based on a LTE-SoC from Marvell which is used for Baseband processing
•	LTE-SoC is a quad core MIPS64 processor with Uplink/Downlink hardware accelerators
•	TRX module has below memory options:
  --	4GB DDR3L as RAM memory
  --	128MB NOR Flash as boot storage
  --	32GB eMMC as main storage
  --	Two 256Kb EEPROMs for configuration and inventory purposes

•	TRX module will have a default configuration switch that will enable the user to reset the Tower Node to factory settings.
•	A push button reset provision is provided on TRX module for user to perform system reset or to reset the Tower Node to factory defaults state.
•	TRX module will have several temperature sensors to enable measuring of temperature near the hot spots.
•	There are four RGB LEDs provided to indicate the status of the system as per implementation. 
•	TRX module is protected for over voltage, under voltage, over current, ESD
•	A generic GPS module is accommodated for timing requirements. 
•	Optional Mask module can be connected to TRX module via mask connector based on the system requirements. Different types of Mask module can be used such as Connectivity, Storage etc.
