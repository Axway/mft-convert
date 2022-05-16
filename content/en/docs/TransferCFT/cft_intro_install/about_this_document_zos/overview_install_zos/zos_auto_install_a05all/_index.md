---

    title: Standard installation (A05ALL )
    linkTitle: Standand installation (A05ALL )
    weight: 210

---
In this section you run A05ALL for a standard installation.

## Use A05ALL to perform an auto install

The A05ALL JCL automatically runs the following members.

QQQ\_QQQ\_CHECK one table subdivided

### Steps relating to {{< TransferCFT/axwayvariablesComponentLongName  >}}


| Step  | Description  | Member  |
| --- | --- | --- |
| 1  | Create and initialize instance files: UCONF, UCONFRUN, FTEST, MONLOG, and LOG.  | A06FILES  |
| 2  | Assemble and link-edit SGINSTAL for Transfer CFT z/OS options.<br/> Non-SMP/E installation mode. &lt;/p&gt; | A12AOPTS  |
| 3  | Link-edit all Transfer CFT modules.<br/> Non-SMP/E installation mode. &lt;/p&gt; | B20LINK  |
| 4  | Generate an encryption key. See also <a href="../t_customize_instance_zos#Password">Password encryption</a>.  | CFTGNKEY  |
| 5.A<br/> OR | Depending on the value of **cgenable** , executes either 5.A or 5.B.<br/> If cgenable is set to **no**:<br/> • Sets the uconf variables: cft.runtime_dir, cft.listcat_compat, cft.state_compat<br/> • Generates the SAMPLE(CFTPARM) parameter sample from the SAMPLE(ZCFTPARM) template<br/> • Generates the SFTP SAMPLE(CFTSFTP) parameter sample from the SAMPLE(ZCFTSFTP) template<br/> • Skips to step 6 | CFT$SET  |
| 5.B  | If cgenable is set to **yes**:<br/> • Sets the uconf variables for Central Governance<br/> • Generates the Transfer CFT parameters sample from a template | CFT$SETC  |
| 6  | Create Transfer CFT files (Parm, Part, Catalog, Com(file), account). | D40INIT  |
| 7  | Loads the Transfer CFT default settings. | E50PARM  |


### Steps relating to PKI


| Step  | Description  | Member  |
| --- | --- | --- |
| 9  | Creates Data base PKI using the sample certificates. | D43PKI  |
| 10  | If pkitype=passport: Enable PassPort PKI configuration. | D44PASS  |
| 11  | If pkitype=system: Enable the PKI system. | D47SYST  |


### Steps relating to Central Governance


| Step  | Description  | Member  |
| --- | --- | --- |
| 15  | If cgenable=yes:<br/> Create the Axway {{< TransferCFT/PrimaryCGorUM  >}} Demonstration Root Certificate. | CFTCGPKI  |
| 16  | If cgenable=yes:<br/> Transfer CFT to {{< TransferCFT/PrimaryCGorUM  >}} registration. | CFTCGREG  |


### Steps relating to Sentinel


| Step  | Description  | Member  |
| --- | --- | --- |
| 20  | If snenable = yes: Sentinel parameters are activated (TCP configuration).<br/> <blockquote> **Note**<br/> This JCL does not create Sentinel tracker logstream.<br/> </blockquote>  | SN05CONF  |
| 25  | If sntlgcre = yes:<br/> Delete /define log stream – DASDONLY. | SN10CLGR  |


### Steps relating to Copilot


| Step  | Description  | Member  |
| --- | --- | --- |
| 30  | If copenable=yes:<br/> Copilot files are transferred in the USS environment (HFS/ZFS). | COPA010  |
| 31  | If copenable=yes:<br/> Updates Copilot UCONF parameters. | COPA020  |
| 32  | REST API configuration.  | RESTCFG  |


### Steps relating to Secure Relay


| Step  | Description  | Member  |
| --- | --- | --- |
| 40  | If Secure Relay is enabled:<br/> Install on USS environment, the files for Secure Relay Master Agent. | XSRA010  |
| 41  | If Secure Relay enabled:<br/> Create the runtime directory for the Transfer CFT Secure Relay Master Agent. | XSRA015  |
| 42  | If Secure Relay is enabled:<br/> Updating Secure Relay parameters (UCONF) | XSRA020  |
| 45  | Configure SAML.  | CFTSAML  |


### Steps relating to Optional steps


| Step  | Description  | Member  |
| --- | --- | --- |
| 50  | Exits and API(s) samples compilation (ASM, C and COBOL).<br/> For COBOL, C: variable LNGPRFX must be customized (I91APICP). | I91APICP  |
| 51  | API(s) link-edit.  | I92APILK  |
| 52  | Exits (ASM, C, and COBOL) link-edit.  | LINKEXLE  |
| 60  | Activate the Transfer CFT heartbeat.  | CFTHEART  |
