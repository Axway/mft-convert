{
    "title": "Post-registration operations",
    "linkTitle": "Post-registration operations ",
    "weight": "190"
}In addition to registration, there are other types of recurring exchanges that occur between {{< TransferCFT/axwayvariablesComponentShortName  >}} and {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}}.

- Configuration and flow deployment
- SSL certificate renewal
- Product updates

Configuration and flow deployment
---------------------------------

From {{< TransferCFT/suitevariablesFlowManager  >}} or {{< TransferCFT/suitevariablesCentralGovernanceName  >}} you can modify Transfer CFT configuration and create flows. Deploying these pushes these modifications (for example, the equivalent of a uconfset command for a configuration change) to the Transfer CFT.

<span id="SSL"></span>

SSL certificate renewal
-----------------------

Certificate and key renewal can refer to business and/or governance certificates. The request is initiated by the Copilot.

> **Note**
>
> Note: The Transfer CFT must be registered.

### Automatic certificate renewal

When Copilot is started the certificates expiration dates in the PKI base are checked, and a renewal request is scheduled. The request schedule is dependent on the value set in the UCONF parameter `cg.renewal_period, `where the default value is 60 days. This represents the number of days before the certificate renewal process occurs relative to the expiration date. Each certificate has a defined expiration date which may differ, and renewal occurs independent of one another.

****Example****

If uconf `cg.renewal_period `is set to 60 days, the renewal procedure executes 60 days before the certificate expiration.

### Manual certificate renewal

To force a certificate renewal, execute the following commands. To force an immediate renewal, use a date that occurred in the past.

> **Note**
>
> Note: The datetime used for the renewal is in UTC and not local time.

#### Set the following parameter to renew the governance certificate

```
CFTUTIL UCONFSET id=cg.certificate.governance.renewal_datetime, value=YYYYMMDDHHMMSS
```

#### Set the following parameter to renew the business certificate

```
CFTUTIL UCONFSET id=cg.certificate.business.renewal_datetime, value=YYYYMMDDHHMMSS
```

Where YYYYMMDDHHMMSS is the date and time of the renewal. For example, August 7 2019, 12:30 has the value 20190807123000.

<span id="Change"></span>

#### Change the private key length

You can configure the key length for either a governance or business certificate from the default value of 2048 to 4096 either when you install {{< TransferCFT/suitevariablesTransferCFTName  >}}, or post-installation if you have a {{< TransferCFT/suitevariablesTransferCFTName  >}} that is already registered with {{< TransferCFT/suitevariablesCentralGovernanceName  >}} or {{< TransferCFT/suitevariablesFlowManager  >}}.

> **Note**
>
> Caution  
> If you modify the key length of the governance certificate and you use access tokens, please refer to the Access Token and Bearer authentication sections for details before proceeding.

1. Modify the UCONF parameters:  
    ```
    CFTUTIL uconfset id=cg.certificate.governance.key_len, value=4096
    CFTUTIL uconfset id=cg.certificate.business.key_len, value=4096
    ```
1. Trigger a certificate renewal, for example by setting the UCONF `cg.certificate.<type>.renewal_datetime` parameter to a date that has passed (example, "20200101000000").
1. Perform a Transfer CFT and a Copilot restart.
1. To perform a check:  
    ```
    PKIUTIL LISTPKI TYPE=USER, CONTENT=FULL &#124; grep "Private Key type"
    ```
      
    The result should be: `Private Key type  RSA      =  4096 bits`

Product updates
---------------

{{< TransferCFT/suitevariablesFlowManager  >}} or Central Governance can apply updates, including service packs, patches, and version upgrades, remotely to registered Transfer CFTs. Download the update package from the [Axway support website](https://support.axway.com/), and upload it to {{< TransferCFT/suitevariablesFlowManager  >}} or Central Governance. Please refer to [Manage product updates](https://docs.axway.com/bundle/CentralGovernance_113_UsersGuide_allOS_en_HTML5/page/Content/updates/t_update_crud.htm) for details.

> **Note**
>
> Note: Available on Unix and Windows, though this service is not available for multi-node.
