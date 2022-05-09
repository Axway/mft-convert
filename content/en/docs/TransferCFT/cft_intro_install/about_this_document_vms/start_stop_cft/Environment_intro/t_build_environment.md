{
    "title": "Building the Transfer CFT environment",
    "linkTitle": "Build the Transfer CFT environment",
    "weight": "220"
}After installation, you must configure Transfer CFT to declare the:

- Network environment
- Partners with which you wish to perform transfers
- Actions to trigger for each event

****Mandatory files****

Transfer CFT behavior is determined by the contents of two indexed files, the PARAMETER and the PARTNER file. Transfer CFT uses a direct file, the CATALOG, to record the transfer-related information. These three files are mandatory.

****Optional files****

Optional environment files include:

- COMMUNICATION: required to use file-based communication
- LOG: used to record all Transfer CFT events
- ACCOUNT: used to record all information concerning terminated transfers

For more information, refer to the Transfer CFT 3.0.1 User's guide or online help.

Configuration files CFT_SCEN: directory
----------------------------------------

The CFT_SCEN: directory contains a set of basic Transfer CFT configuration files.

- ****cft-tcp.conf****: File containing the sample partners’ configuration
- ****cft-tcp-part.conf****: File containing the sample configuration

To configure Transfer CFT, you must create your own configuration file. For more information, refer to the Transfer CFT online documentation.

To interpret the parameter files, use the following command:

$cftinit cft_scen:cft-tcp.conf cft_scen:cft-tcp-part.conf

This procedure also purges directories, creates the files used by Transfer CFT (PARAMETER, PARTNER, CATALOG, COMMUNICATION, LOG and ACCOUNT), and analyzes the contents of the file passed as a parameter to update the PARAMETER and PARTNER files.
