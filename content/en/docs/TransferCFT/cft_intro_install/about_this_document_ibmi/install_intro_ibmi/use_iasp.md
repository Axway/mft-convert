{
    "title": "Use an independent ASP (optionally)",
    "linkTitle": "Use an IASP (optionally)",
    "weight": "210"
}About independent ASPs (IASP)
-----------------------------

An independent ASP (or independent disk pool) is a collection of disk units that can be brought online or taken offline independent of other system storage, including the system ASP, user ASPs, and other independent ASPs. Independent ASPs are useful both in single and multiple system environments.

In a single system environment, you can take an independent ASP offline independent of other ASPs as the data in the independent ASP is self-contained. This means that all of the necessary system information associated with the independent ASP's data is contained within the independent ASP. You can also bring the independent ASP online while the system is active (no IPL required). This use of independent ASPs can be useful, for example if you have large amounts of data that are not needed for normal day-to-day business processing. The independent ASP containing this data can be left offline until it is needed. When large amounts of storage are systematically kept offline, you can shorten processing time for operations such as IPL and reclaim storage. (IBM)

IASP command
------------

You can use the following commands to manage IASP:

- To start or stop an IASP, use the command:  
    ```
    WRKDEVD
    ```
- To activate an environment, use the command:  
    ```
    SETASPGRP (ex: ‘SETASPGRP ASPGRP(IASP1)’)
    ```
- To disable an environment, use the command:  
    ```
    SETASPGRP ASPGRP(\*NONE)
    ```

Use an IASP in a Transfer CFT environment
-----------------------------------------

For Transfer CFT to be able to access the files located on an Independent ASP (write or read mode), you must have an available IASP environment. To enable this access:

1. Connect to your system.
1. Start the IASP.
1. Before starting Transfer CFT, use the `EDTLIB `command to add the following three libraries:
    -   CFTPGM: Contains the Transfer CFT programs
    -   CFTPROD: Contains the Transfer CFT configuration
    -   CFTSBSLIB: An additional library in the ASP that contains your SBS
1. Use the `SETASPGRP `command to enable your IASP.
1. Use the `CFTSTART `command to start Transfer CFT.
