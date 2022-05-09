{
    "title": "Manually enable Central Governance ",
    "linkTitle": "Manually enable Central Governance",
    "weight": "230"
}This section describes how to manually modify the Transfer CFT configuration to enable Central Governance connectivity in command line.

Prerequisites
-------------

1. Stop Transfer CFT and Copilot if running.
1. Ensure that all UCONF values used to identify a Transfer CFT instance are defined. These parameters include:
    -   cft.full_hostname
    -   cft.instance_id
    -   cft.instance_group

      
    Use the format:  
    ```
    CFTUTIL uconfset id=cft.instance_id, value=<cft_id>
    ```

Procedure
---------

The manual procedure consists of the following steps, which are detailed below:

1. Set the UCONF parameter values for Central Governance.
1. Enable {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.
1. Start Copilot to register.

#### Set UCONF values

Use the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} installation values for the following UCONF settings. {{< TransferCFT/suitevariablesTransferCFTName  >}} uses these values to identify {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

- cg.host
- cg.port
- cg.mutual_auth_port
- cg.shared_secret

Use the format:

```
CFTUTIL uconfset id=cg.host, value=<host_value>
```

#### Enable {{< TransferCFT/suitevariablesCentralGovernanceName  >}}

```
CFTUTIL uconfset id=cg.enable, value=yes
```

#### Register

Start the {{< TransferCFT/suitevariablesTransferCFTName  >}} Copilot to trigger an automatic registration with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

You can check in the {{< TransferCFT/PrimaryCGorUM  >}} **Product List** to confirm that the registration was successful.
