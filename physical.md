Physical Servers
=======

The physical servers are transitioning to a new home, and many/most are going away. These instructions will be updated when the transition is complete.

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
| *                   |                         |                               |                           |                     |
| Tripplite BBU 1     | Tripplite SU2200RTXL2Ua | N/A                           | N/A                       | N/A                 |
| *                   |                         |                               |                           |                     |
| Tripplite BBU 1     | Tripplite SU2200RTXL2Ua | N/A                           | N/A                       | N/A                 |
| *                   |                         |                               |                           |                     | 
| Tripplite Ext. Batt |                         | N/A                           | N/A                       | N/A                 |
| *                   |                         |                               |                           |                     | 

   
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

Manuals
---------

If at any point you are concerned about the steps required to perform physical maintenance, visit the online PDF manuals for the hardware we've acquired:

  * [Supermicro 6027R-WRF](http://www.supermicro.com/manuals/superserver/2U/MNL-1350.pdf)
  * [Supermicro 6047R-E1R36L](http://www.supermicro.com/manuals/superserver/4U/MNL-6047R-E1R36L.pdf)
