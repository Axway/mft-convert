{
    "title": "updat",
    "linkTitle": "updat",
    "weight": "3670"
}<span id="updat"></span>

### updat

#### CFTCAT

**[UPDAT = {256
&#124; n}]  **{0..32767}

Number of synchronization points between two consecutive updates of
the catalog file during a transfer.

Update allows the number of records sent and acknowledged for a given
transfer to be written to the disk.

For example:

- 0 means the catalog
    file is not updated during transfer
- 1 means the catalog
    file is updated at each synchronization point

*Note: For* some systems, increasing
this update number is likely to increase the CPU and I/O overhead and
degrade transfer throughputs.

 

[Return to Command index](../../)
