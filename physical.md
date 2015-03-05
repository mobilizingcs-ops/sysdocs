Physical Servers
=======

The CENS server closet currently houses our rack of servers in addition to a second rack used by the ScAI Institute folks. Given the right set of barn-doors, our rack is the TRIPPLITE rack furthest to the left.

Stack Order
-----------

| Server              | Model                   | CPU                           | Memory                    | Disk                |
| --------------------| ----------------------- | ----------------------------- | --------                  | -----               |
| starbuck.ohmage.org | Supermicro 6047R-E1R36L | 2x Intel Xeon E3-2609 2.4ghz  | 96GB (6x16GB DDR3-1600Mhz)| 16x2TB, 2x240GB SSD |
| *                   |                         |                               |                           |                     |
| *                   |                         |                               |                           |                     |
| *                   |                         |                               |                           |                     |
| athena.ohmage.org   | Supermicro 6027R-WRF    | 2x Intel Xeon E5-2690, 2.9ghz | 256GB (16x16GB DDR3)      | 8x2TB (RAID10)      |
| *                   |                         |                               |                           |                     |
| boomer.ohmage.org   | Supermicro 6027R-WRF    | 2x Intel Xeon E5-2690, 2.9ghz | 256GB (16x16GB DDR3)      | 8x2TB (RAID10)      |
| *                   |                         |                               |                           |                     |
| eight.ohmage.org    | Sun X4275               | 2x Intel Xeon X5560, 2.8ghz   | 88G                       | 12x1TB (RAID10)     |
| *                   |                         |                               |                           |                     | 
| cavil.ohmage.org    | Dell PowerEdge 2950III  | 2x Intel Xeon E5440, 2.8ghz   | 20GB                      | 4x2TB (RAIDZ)       |       
| *                   |                         |                               |                           |                     |
| doral.ohmage.org    | Dell PowerEdge 2950III  | 2x Intel Xeon E5440, 2.8ghz   | 32GB                      | 4x146GB (RAID10)    |         
| *                   |                         |                               |                           |                     |
| Tripplite BBU 1     | Tripplite SU2200RTXL2Ua | N/A                           | N/A                       | N/A                 |
| *                   |                         |                               |                           |                     |
| Tripplite BBU 1     | Tripplite SU2200RTXL2Ua | N/A                           | N/A                       | N/A                 |
| *                   |                         |                               |                           |                     | 
| Tripplite Ext. Batt |                         | N/A                           | N/A                       | N/A                 |
| *                   |                         |                               |                           |                     | 

We also maintain 2 of the Cisco 3750G switches on the switch stack for this infrastructure.  Trace the cable run back to determine which two (more info on this later.)
   
Maintenance
-----------

It's always best not to perform maintenance while the systems are running, but unless the circumstances are dire, do not simply press the power button to turn off a given server. Some other things to note
  
  * Removal of a hot-swap drive bay should be done with extreme caution, and only after verifying the disk is indeed failed or the system is in a powered-off state
  * The redundant power supplies in the Supermicro servers can safely be pulled from a running server.
    * Ensure the power supply you are replacing has had it's failed light illuminated (system will likely also be making a continuous beep tone)
    * Disconnect the power cable from the supply.
    * Flip the lever to release the supply from the chassis.
    * Ensure back clearance as you **slowly** remove (this will be tight).
  * If ram, cpu, fans or any other hardware needs maintenance please first power off the box.

The monitor/keyboard sitting on starbuck.ohmage.org can be used to access the console on any of these hosts if needed.  Visit the back of the rack (each server is labeled on both front and back) and connect the monitor and keyboard to the server in question. 

Manuals
---------

If at any point you are concerned about the steps required to perform physical maintenance, visit the online PDF manuals for the hardware we've acquired:

  * [Supermicro 6027R-WRF](http://www.supermicro.com/manuals/superserver/2U/MNL-1350.pdf)
  * [Supermicro 6047R-E1R36L](http://www.supermicro.com/manuals/superserver/4U/MNL-6047R-E1R36L.pdf)
  * [Dell PowerEdge 2950III](http://downloads.dell.com/Manuals/all-products/esuprt_ser_stor_net/esuprt_poweredge/poweredge-2950_owner%27s%20manual_en-us.pdf)
  * [Sun X4275](https://docs.oracle.com/cd/E19871-01/820-5830-13/820-5830-13.pdf)
  * [Cisco 3750G](http://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst3750/hardware/installation/guide/3750hig.pdf)