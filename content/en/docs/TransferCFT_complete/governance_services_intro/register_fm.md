{
    "title": "Register Transfer CFT with Flow Manager",
    "linkTitle": "Register with Flow Manager",
    "weight": "170"
}This section describes how to register a Transfer CFT instance with either an on premise or SaaS {{< TransferCFT/suitevariablesFlowManager  >}}. You can refer to the [Flow Manager User Guide](https://docs.axway.com/bundle/FlowManager_20_allOS_en_HTML5/page/user_guide.html) for Flow Manager details.

Prerequisites
-------------

****{{< TransferCFT/axwayvariablesComponentLongName  >}} prerequisites****

- An installed Transfer CFT version 3.6 or higher on a compatible platform.
- Transfer CFT and Copilot services are stopped.

****{{< TransferCFT/suitevariablesFlowManager  >}} **prerequisites******

- The shared secret that the Flow Manager administrator generated. Refer to the [Flow Manager User Guide](https://docs.axway.com/bundle/FlowManager_20_allOS_en_HTML5/page/flow_manager_user_guide.html) for details.
- If you are implementing a SaaS cloud Flow Manager, you additionally require:
    -   A Flow Manager Agent capable of interconnecting Flow Manager and your {{< TransferCFT/axwayvariablesComponentLongName  >}} instance. Refer to the [Flow Manager User Guide](https://docs.axway.com/bundle/FlowManager_20_allOS_en_HTML5/page/user_guide.html) for details.
    -   Access to an on-demand Flow Manager SaaS environment.

Automatically activate connectivity
-----------------------------------

****UNIX/Windows****

The automatic activation is only available on UNIX and Windows platforms and can be done during the installation. Please refer to the OS specific *Transfer CFT Installation Guide &gt; Customize the initialize.properties file &gt; Central Governance* section.

Manually activate connectivity
------------------------------

This section describes the steps to register your {{< TransferCFT/axwayvariablesComponentLongName  >}} with {{< TransferCFT/suitevariablesFlowManager  >}}. The procedure is the same for either an on-premise or SaaS {{< TransferCFT/suitevariablesFlowManager  >}}, with the exception of the two steps described in [Define the {{< TransferCFT/suitevariablesFlowManager  >}} Agent](#Define).

All commands in this section are performed using CFTUTIL unless stated otherwise. For details on the UCONF parameters referenced in this section, please see [UCONF: Central Governance options]().

#### Define UCONF parameters used for {{< TransferCFT/axwayvariablesComponentLongName  >}} instance identification

Set the parameters used to identify a Transfer CFT instance. Follow these guidelines, otherwise the registration will fail:

- The length of the `cft.instance_id` value is limited to 24 characters.
- The address set in `cft.full_hostname` must be reachable from {{< TransferCFT/suitevariablesFlowManager  >}} or a Flow Manager Agent (for a SaaS deployment).

```
uconfset id=cft.instance_id, value=<cft_id>
uconfset id=cft.instance_group, value=<cft_instance_group>
uconfset id=cft.full_hostname, value=<cft_address>
```

Additionally, if running in a multi-host/multi-node environment, you must set the load balancer address(FQDN or IP address) and port that {{< TransferCFT/suitevariablesFlowManager  >}} uses to reach the Transfer CFT (`copilot.general.ssl_serverport`):

```
uconfset id=cft.multi_node.load_balancer.host, value=<load_balancer_address>
uconfset id= cft.multi_node.load_balancer.port,value=<load_balancer_port>
```

#### Define the Flow Manager Agent for SaaS

Define the name of the Flow Manager Agent that the Flow Manager must use to connect with your Transfer CFT instance.

```
uconfset id=cg.metadata.agent.value, value=<agent_host_FQDN>
```

Please refer to the [Flow Manager User Guide](https://docs.axway.com/bundle/FlowManager_20_allOS_en_HTML5/page/user_guide.html) for Flow Manager Agent details.

#### Optionally define a proxy server for on-premise {{< TransferCFT/suitevariablesFlowManager  >}} to {{< TransferCFT/axwayvariablesComponentLongName  >}} communication

To use a proxy server for your on-premise {{< TransferCFT/suitevariablesFlowManager  >}} to connect to {{< TransferCFT/axwayvariablesComponentLongName  >}}, set the following parameters.

```
uconfset id=cg.proxy.in.host, value= <proxy_address>
uconfset id=cg.proxy.in.port ,value= <proxy_port>
uconfset id=cg.proxy.in.login, value= <proxy_login>
uconfset id= cg.proxy.in.password, value= <proxy_login_password>
```

#### Optionally define a proxy server for {{< TransferCFT/axwayvariablesComponentLongName  >}} to {{< TransferCFT/suitevariablesFlowManager  >}} communication

To use a proxy server for your {{< TransferCFT/axwayvariablesComponentLongName  >}} to connect to {{< TransferCFT/suitevariablesFlowManager  >}}, set the following parameters.

```
uconfset id=cg.proxy.out.host, value= <proxy_address>
uconfset id=cg.proxy.out.port,value= <proxy_port>
uconfset id=cg.proxy.out.login, value= <proxy_login>
uconfset id=cg.proxy.out.password, value= <proxy_login_password>
```

> **Note**
>
> Note: Transfer CFT can use the Flow Manager Agent as the outgoing HTTP proxy when connecting to the Flow Manager SaaS.

#### Import the root certificate for the {{< TransferCFT/suitevariablesFlowManager  >}} certificate

Before Transfer CFT can register with Flow Manager, the HTTPS root certificate's CA must be known and trusted by the registering Transfer CFT.

1. Download the HTTPS root certificate's CA, which is used to authenticate {{< TransferCFT/suitevariablesFlowManager  >}}.
1. Import this root CA into the PKI database using the PKIUTIL PKICER command.
1. Set the `iname `to the root CA path.
1. Define the UCONF variable `cg.ca_cert_id`, which must correspond with the value you set in the previous step. It is required so that {{< TransferCFT/suitevariablesTransferCFTName  >}} knows which certificate to use to authenticate {{< TransferCFT/suitevariablesFlowManager  >}}. Using CFTUTIL:

#### Define the parameters used for the {{< TransferCFT/suitevariablesFlowManager  >}} connection

Set the following parameters that are used to connect to {{< TransferCFT/suitevariablesFlowManager  >}}.

```
uconfset id=cg.host, value=<Flow_Manager_FQDN>
uconfset id=cg.port, value=<FM_port>
```

Set the shared secret that the Flow Manager administrator generated and provided.

```
uconfset id=cg.shared_secret, value=<Shared_Secret>
```

> **Note**
>
> Note: If the shared secret is incorrect, Transfer CFT may be rejected with a 403 HTTP code. If this happens, the cg.enable mode becomes disabled on your Transfer CFT to avoid sending repeat requests to Flow Manager.

#### Optionally define the configuration policy for registration

You may want to automatically assign an existing {{< TransferCFT/suitevariablesFlowManager  >}} configuration policy during the {{< TransferCFT/axwayvariablesComponentLongName  >}} registration. To do so, set the UCONF parameter `cg.configuration_policy` to the name of the desired policy.

```
uconfset id=cg.configuration_policy, value=<name_of_policy>
```

#### Optionally customize the business certificate Distinguished Name (DN)

You may want to customize the business certificate's Distinguished Name (DN), which is generated during the {{< TransferCFT/suitevariablesFlowManager  >}} registration or certificate renewal. Set the UCONF parameter cg.certificate.business.csr_dn to the custom value. The default is O=Axway,OU=MFT,CN=%uconf:cft.full_hostname%. Remember to separate tokens by a comma.

```
uconfset id=cg.certificate.business.csr_dn, value='O=MyCompany,OU=MFT,CN=%uconf:cft.full_hostname%'
```

A best practice is to customize the certificate DN prior to registration. However, if you are customizing the certificate DN after the Transfer CFT registration, you can force an immediate renewal or wait for the automatic renewal as described in [SSL certificate renewal](../cg_postregister#SSL).

#### Optionally customize the governance certificate Distinguished Name (DN)

To override the governance certificate's Distinguished Name (DN), which is generated during the {{< TransferCFT/suitevariablesFlowManager  >}} registration or certificate renewal, set the UCONF parameter cg.certificate.governance.csr_dn to the custom value. The default is O=Axway,OU=MFT,CN=&lt;Transfer CFT $(cft.instance_id)&gt;. Remember to separate tokens by a comma.

```
uconfset id=cg.certificate.governance.csr_dn, value='O=MyCompany,OU=MFT,CN=%uconf:cft.full_hostname%'
```

A best practice is to customize the certificate DN prior to registration. However, if you are customizing the certificate DN after the Transfer CFT registration, you can force an immediate renewal or wait for the automatic renewal as described in [SSL certificate renewal](../cg_postregister#SSL).

<span id="Customize_keylength"></span>

#### Optionally customize the certificates' key length

By default Transfer CFT generates a key length of 2048 bits for its Governance and Business certificates. Optionally you can modify these values to 4096 bits.

```
uconfset id=cg.certificate.governance.key_len, value=4096
uconfset id=cg.certificate.business.key_len, value=4096
```

#### Enable {{< TransferCFT/suitevariablesFlowManager  >}}

To enable connectivity, enter:

```
uconfset id=cg.enable, value=yes
```

#### Perform the check command to validate parameters

Use the CFTUTIL CHECK command to validate the coherence of parameters, partners, and the Transfer CFT PKI database.

```
CHECK CONTENT=BRIEF&#124;FULL, FOUT=FileName
```

Check the list in the output for errors and correct all errors before attempting registration. See also, [Use the check command](../../c_intro_userinterfaces/about_cftutil/check_command).

Register or re-register
-----------------------

Ensure that `cft_registration_id `is reset to `-1`. Otherwise, reset it as follows:  

```
CFTUTIL uconfunset id=cg.registration_id
```

Start the {{< TransferCFT/suitevariablesTransferCFTName  >}} Copilot to automatically trigger registration. From the Flow Manager UI, check the **Product List** to confirm that the registration was successful.
