{
    "title": "3. Define client user rights ",
    "linkTitle": "3. Define client user rights",
    "weight": "220"
}This section describes the `copilot.misc.createprocessasuser` parameter.

****When using {{< TransferCFT/suitevariablesCentralGovernanceName  >}}****

By default this parameter is set to NO and user authentication is controlled by {{< TransferCFT/PrimaryCGorUM  >}}. Setting this to YES allows system user authentication via a client, such as web services, if there were a Central Governance failure. This setup requires that users be known on both the system and on {{< TransferCFT/PrimaryCGorUM  >}}, meaning an LDAP directory.

> **Note**
>
> Note: Configuration changes should be managed by Central Governance.

****When using standalone {{< TransferCFT/suitevariablesTransferCFTName  >}}****

When set to YES, user authentication is controlled by the system where Transfer CFT is installed and the Transfer CFT Copilot server starts a process under the connected user. Note that the default value is platform specific. When set to NO, actions made on the configuration are done with the user that **started** the Transfer CFT Copilot server.


| OS  | Default  |
| --- | --- |
| Unix | NO  |
| Windows | YES  |
| IBM i  | YES  |
| z/OS  | This functionality was modified in Transfer CFT 3.2.4 SP1:<br/> • Post-SP1: The default value for <code>createprocessasuser </code>is YES.<br/> • Pre-SP1: There is no definable value. The equivalent of <code>createprocessasuser </code>depends on the use of APF. If JOBLIB is not defined as an APF, it is the equivalent of NO. If defined, this is the equivalent of YES. |


> **Note**
>
> Note: Using Central Governance you must set appropriate user privileges. Otherwise, a user with extensive Central Governance privileges could modify the Transfer CFT configuration by connecting via the client, even if this user has restricted access to the runtime environment.

When createprocessasuser is set to YES, you must perform the OS specific tasks as described in the appropriate Installation Guide.

- Refer to the {{< TransferCFT/suitevariablesTransferCFTName  >}} {{< TransferCFT/axwayvariablesComponentVersion  >}} *Installation Guide Unix&gt; Unix operations &gt; Using system users* for detailed instructions.
- Refer to the {{< TransferCFT/suitevariablesTransferCFTName  >}}{{< TransferCFT/axwayvariablesComponentVersion  >}} *Installation Guide Windows &gt; Windows operations &gt; Using system users* for detailed instructions.

****Related topics****

- [About system users](../)
- [User rights use case scenarios](../user_rights_security_scenarios)
- [Recommendations and troubleshooting](../user_rights_tips)
