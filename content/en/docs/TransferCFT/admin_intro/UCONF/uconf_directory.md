{
    "title": "UCONF parameters",
    "linkTitle": "UCONF parameters",
    "weight": "260"
}You can download this page as a PDF here.

****Viewing the table****

If you have trouble viewing the entire table in your browser, click the arrow icon to collapse the left pane TOC.

![](/Images/TransferCFT/collapse.png)

****Parameter attributes in lower case****

Some uconf values contain logical_name parameter attributes as part of the value, for example `aws.credentials.<logical_name>.access_key_id`. When processed, the logical_name is replaced by the parent value, and is systematically converted to lower case.

Example when `aws.credientials=Test`

`CFTUTIL listuconf id=aws.credentials.*`

`aws.credentials = Test`

`aws.credentials.test.access_key_id =`

`aws.credentials.test.secret_access_key = ********`

****Parameter list****

The following table is an exhaustive list of the unified configuration (UCONF) values.

> **Note**
>
> Caution  
> Do not modify the UCONF dictionary located in the home directory unless specifically directed to do so by Axway.

{{% TransferCFT/snippets/uconf_flags%}}


| Name | Description | Type | Restriction | Default Value | Platform | Flags |
| --- | --- | --- | --- | --- | --- | --- |
| acceleration.enable | Enables the acceleration. | bool Yes No |   | No | &quot;NT, UNIX&quot; | EXPERT |
| acceleration.ptcp | List of CFTNET objects that implement the pTCP acceleration. | string list |   |   | ALL |   |
| acceleration.ptcp.&lt;logical_name&gt;.buffer_size | Internal acceleration buffer size in bytes. | int |   | 10240000 | ALL |   |
| acceleration.ptcp.&lt;logical_name&gt;.nb_connections | Number of parallel connections. | int |   | 10 | ALL |   |
| acceleration.ptcp.&lt;logical_name&gt;.packet_size | TCP packet size in bytes. | int |   | 3000 | ALL |   |
| acceleration.ptcp.&lt;logical_name&gt;.session_affinity_delay | &quot;Dispatches to the same node within this delay, in milliseconds (multinode only).&quot; | int |   | 0 | ALL |   |
| acceleration.ptcp.&lt;logical_name&gt;.session_init_timeout | &quot;Determines the timeout value, in milliseconds, for establishing pTCP session.&quot; | int |   | 5000 | ALL |   |
| acceleration.udt | List of CFTNET objects that implement the UDT acceleration. | string list |   |   | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.buffer_size | Internal UDT buffer size in bytes. | int |   | 10240000 | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.fc | Maximum window size in packets. | int |   | 25600 | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.linger | Linger time on close() call. | int |   | 180 | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.max_bw | Maximum bandwidth that one single UDT connection can use in bytes/second. -1 means unlimited. | int |   | -1 | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.mss | Maximum packet size in bytes. | int |   | 1500 | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.rcv_buf | UDT receiver buffer size limit in bytes. | int |   | 10240000 | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.snd_buf | UDT sender buffer size limit in bytes. | int |   | 10240000 | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.udp_rcv_buf | UDP socket receiver buffer size in bytes. | int |   | 1024000 | ALL |   |
| acceleration.udt.&lt;logical_name&gt;.udp_snd_buf | UDP socket sender buffer size in bytes. | int |   | 1024000 | ALL |   |
| am.cg.organization | Default Organization when CG is the Access Management type used. | string | &quot;min length:0, max length:35&quot; | '' | ALL |   |
| am.exit.check_login | Checks login with Access Management exit. | bool Yes No |   | Yes | ALL |   |
| am.exit.check_permissions | Checks permissions with Access Management exit. | bool Yes No |   | Yes | ALL |   |
| am.exit.custom | Customer configuration parameters available in customizable Access Management exit. | string list |   | tracelevel rbac_fname ldap_host ldap_port ldap_base ldap_login_dn_format ldap_get_roles_filter (not MVS) &#124; tracelevel rbac_fname ldap_host ldap_port ldap_base ldap_login_dn_format ldap_get_roles_filter safclass instance_id (MVS) | ALL |   |
| am.exit.libpath | Location of Access Management exit library. | fname | &quot;min length:0, max length:512&quot; | &quot;none (not MVS, not VMS) &#124; CFTDXAM (MVS) &#124; CFT_EXE:AM_EXIT.EXE (VMS)&quot; | ALL |   |
| am.internal.group_database | Group database where group members are defined. | string |   | 'system' (not MVS) &#124; 'file' (MVS) | &quot;NT, UNIX, MVS, OS400, VMS&quot; |   |
| am.internal.group_database.fname | The file where group members are defined. Used only when am.internal.group_database equals file. | fname | &quot;min length:0, max length:512&quot; | $(cft.mvs.group_fname) (MVS) | &quot;MVS, VMS&quot; |   |
| am.internal.libpath | Location of Private Access Management exit library. Do not modify this parameter! | fname | &quot;min length:0, max length:512&quot; | $(cft.install_dir)/lib/libcftpexam.so (UNIX) &#124; $(cft.install_dir)/lib/libcftpexam.a (AIX) &#124; $(cft.install_dir)/lib/libcftpexam.sl (HPUX-PARISC) &#124; $(cft.install_dir)/bin/cftpexam.dll (NT) &#124; CFTDXIN (MVS) &#124; LIBCFTPEXA (OS400) &#124; CFT_EXE:AMTYPE_INTERNAL.EXE (VMS) | &quot;NT, UNIX, MVS, OS400, VMS&quot; | EXPERT |
| am.internal.persistence_timeout | Delay in seconds between updating the list of groups that a user belongs to. | int |   | 300 | &quot;NT, UNIX, MVS, OS400, VMS&quot; |   |
| am.internal.role.admin | Admin role and groups mapping. This role enables you to perform all administrative tasks. | string list |   | ADMIN (MVS) | &quot;NT, UNIX, MVS, OS400, VMS&quot; |   |
| am.internal.role.application | Application role and groups mapping. This role enables application to send transfers. | string list |   | TRANSFER (MVS) | &quot;NT, UNIX, MVS, OS400, VMS&quot; |   |
| am.internal.role.designer | Designer role and groups mapping. This role enables you to manage flows. | string list |   | DESIGNER (MVS) | &quot;NT, UNIX, MVS, OS400, VMS&quot; |   |
| am.internal.role.helpdesk | &quot;Help Desk role and groups mapping. This role enables you to view the log, transfers and configuration.&quot; | string list |   | OPERATOR (MVS) | &quot;NT, UNIX, MVS, OS400, VMS&quot; |   |
| am.internal.role.partnermanager | Partner Manager role and groups mapping. This role enables you to create and manage partners. | string list |   | PARTNER (MVS) | &quot;NT, UNIX, MVS, OS400, VMS&quot; |   |
| am.internal.safclass | The specified class must be defined in the class descriptor table. | string |   | '' | MVS |   |
| am.passport.ca_cert | List of Certification Authority (CA) public certificates to authenticate PassPort AM server. | string list |   | &quot;$(cft.runtime.confpki_dir)passportCA.pem (not MVS, not OS400) &#124; $(cft.runtime.confpki_dir)CERTIF(PAMCAPEM) (MVS) &#124; *LIBL/PASSPRTPEM (OS400)&quot; | ALL |   |
| am.passport.cg.organization | Name of the Central Governance Organization in Passport AM. | string |   | '' | ALL |   |
| am.passport.connection_timeout | Connection timeout to PassPort AM. | int |   | 120 | ALL |   |
| am.passport.csd_file | Transfer CFT Component Security Descriptor file for PassPort AM. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.install.extrasPS_dir)csd_Transfer_CFT_CG.xml (not MVS, not OS400) &#124; $(cft.install.extrasPS_dir)XMLLIB(CSDCG) (MVS) &#124; *LIBL/CSDCFTCG (OS400)&quot; | ALL |   |
| am.passport.domain | PassPort AM domain. | string |   | 'Synchrony' | ALL |   |
| am.passport.hostname | PassPort AM server hostname or IP address. | string |   | 'localhost' | ALL |   |
| am.passport.instance_id | Application instance ID. | string |   | 'default' &#124; '$(cft.instance_group).$(cft.instance_id)' (VMS) | ALL |   |
| am.passport.login | Passport AM application login. | string |   | '' | ALL |   |
| am.passport.max_connections | Maximum number of PassPort AM connections. | string |   | '1' | ALL | EXPERT |
| am.passport.password | PassPort AM application password. | passwd |   |   | ALL |   |
| am.passport.persistency.cftsxpam.enable | Enable execution of CFTSXPAM that updates the Passport AM cache. | bool Yes No |   | Yes | ALL |   |
| am.passport.persistency.check_interval | Interval in seconds between two checks of access management updates. | int |   | 600 | ALL |   |
| am.passport.persistency.enable | Enables persistency support for PassPort AM. | bool Yes No |   | Yes | ALL |   |
| am.passport.persistency.fname | Persistent cache file name for PassPort AM. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/data/CFTAM (not MVS, not OS400, not VMS) &#124; $(cft.runtime_dir)DATA]CFTAM (VMS) &#124; $(cft.runtime_dir)CFTAM (MVS) &#124; *LIBL/CFTAM (OS400)&quot; | ALL |   |
| am.passport.persistency.lock_fname | Persistent cache lock file name for PassPort AM. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/data/CFTAM.lck (not VMS, not OS400) &#124; $(cft.install_dir)data]CFTAM.lck (VMS) &#124; *LIBL/CFTAMLCK (OS400)&quot; | ALL |   |
| am.passport.pipe_priority | &quot;Pipelining priority mode (1 pipeline priority, 0 new connection priority).&quot; | string |   | '0' | ALL | EXPERT |
| am.passport.pipeline_size | Maximum number of requests in pipe for a single PassPort AM connection. | string |   | '2' | ALL | EXPERT |
| am.passport.port | PassPort AM server port number. | int |   | 6090 | ALL |   |
| am.passport.resource_prefix | PassPort AM resource prefix. | string |   | 'Synchrony:TransferCFT' | ALL | EXPERT |
| am.passport.srchost | Local network interface used by the AM Passport connector for outgoing connection. | string |   | ' ' | ALL |   |
| am.passport.superuser | Users for which no PassPort AM rights check is done. | string list |   | $(CFT$USER) (VMS) | ALL |   |
| am.passport.trace | Passport AM trace level (from 0 to 5). | string |   | '0' | ALL |   |
| am.passport.use_ssl | Enable SSL cryptography when connecting to PassPort AM server. | bool Yes No |   | No | ALL |   |
| am.passport.userctrl.check_permissions_on_transfer_execution | Check permissions for CREATE and EXECUTE actions on the TRANSFER resource when the system user control is enabled (CFTPARM USERCTRL=YES). | bool Yes No |   | Yes | ALL |   |
| am.persistency.fname | &quot;Persistent users and roles cache file name for AM with CG, Keycloak, LDAP&quot; | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/data/CFTAMUR (not MVS, not OS400, not VMS) &#124; $(cft.runtime_dir)DATA]CFTAMUR (VMS) &#124; $(cft.runtime_dir)CFTAMUR (MVS) &#124; *LIBL/CFTAMUR (OS400)&quot; | ALL |   |
| am.saml.client_id | Value used as issuer on SAML requests. This should match the Identity Provider configuration. | string |   | '$(cft.instance_id)' | ALL |   |
| am.saml.idp.cert_id | Identifier of the certificate (stored in the internal PKI base) used to verify the SAML Identity Provider server's signatures. | string |   | ' ' | ALL |   |
| am.saml.idp.logout_service | Endpoint for SAML requests of type LogoutRequest (HTTP-Redirect binding).\ | string |   | '$(am.saml.idp.sign_on_service)' | ALL |   |
| am.saml.idp.sign_on_service | Endpoint for SAML requests of type AuthnRequest (HTTP-Redirect binding).\ | string |   | ' ' | ALL |   |
| am.superuser | Users for which no CG AM rights check is done. | string list |   | $(am.passport.superuser) (not VMS) &#124; $(CFT$USER) (VMS) | ALL |   |
| am.type | Access Management type used. | string | enum: none cft passport exit internal cg fc saml | 'none' | ALL |   |
| aws.credentials | List of parameters for AWS. | string list |   |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.access_key_id | &quot;Access Key ID (a 20-character, alphanumeric sequence).&quot; | string |   |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.acl | ACL policy to apply on the created file while uploading a file to AWS. | string | enum: private public-read public-read-write authenticated-read aws-exec-read bucket-owner-read bucket-owner-full-control |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.encryption_key_id | Key identifier to use for the SSE-KMS encryption. | passwd |   |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.encryption_type | &quot;Server side encryption type. If empty, there is no encryption.&quot; | string | enum: none sse-s3 sse-kms | none | ALL |   |
| aws.credentials.&lt;logical_name&gt;.proxy_host | Proxy IP address or FQDN. | string |   |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.proxy_password | Proxy password. | passwd |   |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.proxy_port | Proxy port. | int |   |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.proxy_scheme | Proxy scheme to use. | string | enum: http https |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.proxy_username | Proxy user name. | string |   |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.region | &quot;Region to use, overrides parsing result of WORKINGDIR and config/env settings.&quot; | string |   |   | ALL |   |
| aws.credentials.&lt;logical_name&gt;.secret_access_key | Secret Access Key (a 40-character sequence). | passwd |   |   | ALL |   |
| cft.accnt.enable_extended_byte_fields | Enables fields FBYTE_EXTENDED and NBYTE_EXTENDED (length=15) of the Accounting file. | bool No Yes |   | No | ALL |   |
| cft.api.catalog.cache_size | Size of the catalog cache for API calls. | int |   | 20 | ALL |   |
| cft.api.waitcat.scantime_scale | Scantime scale (1/n seconds). | int |   | 1 | ALL |   |
| cft.cft_charsets | Adds a character set ID to the existing list of charsets. | string list |   |   | ALL | IRECONFIG |
| cft.cftaccnt.afname | Overrides the CFTACNTA logical name used for the alternate Accounting file. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.accnt_dir)cftaacnt (not VMS, not MVS) &#124; $(cft.runtime.accnt_dir)cftaccnt.loga (VMS) &#124; $(cft.runtime.accnt_dir)ACCNT2 (MVS)&quot; | ALL |   |
| cft.cftaccnt.afname.atts | Attributes of the alternate Accounting file. | string |   | &quot;'$(cft.cftaccnt.fname.atts)' (MVS, OS400)&quot; | &quot;MVS, OS400&quot; | EXPERT |
| cft.cftaccnt.fname | Overrides the CFTACNT logical name used for the Accounting file. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.accnt_dir)cftaccnt (not VMS, not MVS) &#124; $(cft.runtime.accnt_dir)cftaccnt.log (VMS) &#124; $(cft.runtime.accnt_dir)ACCNT1 (MVS)&quot; | ALL |   |
| cft.cftaccnt.fname.atts | Attributes of the Accounting file. | string |   | &quot;'format=v24' (OS400, VMS) &#124; 'format=v24,fblksize=14336,fspace=3000' (MVS)&quot; | &quot;MVS, OS400, VMS&quot; | EXPERT |
| cft.cftcat.auto_expand_max_size | Maximum size in record for automatic expansion. | int |   | 1000000 | ALL | RECONFIG |
| cft.cftcat.auto_expand_percent | Expansion factor. | int |   | 0 | ALL | RECONFIG |
| cft.cftcat.default_size | Default size in records used when creating a catalog with cftinit. | int |   | 10000 | ALL |   |
| cft.cftcat.enable_deprecated_blknum | &quot;Enables BLKNUM as a catalog management command parameter (LISTCAT, DELETE, START, etc.). Do not enable BLKNUM when using the multi-node feature.&quot; | bool No Yes |   | No | ALL |   |
| cft.cftcat.fname | Replaces the CFTCATA logical name. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.data_dir)cftcata (not VMS, not MVS) &#124; $(cft.runtime.data_dir)CFTCATA.REL (VMS) &#124; $(cft.runtime.data_dir)CATALOG (MVS)&quot; | ALL |   |
| cft.cftcom.default_size | Default size used when creating a COM file with cftinit. | int |   | 1000 | ALL |   |
| cft.cftcom.fname | Overrides the CFTCOM logical name. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.data_dir)cftcom (not VMS, not MVS) &#124; $(cft.runtime.data_dir)cftcom.rel (VMS) &#124; $(cft.runtime.data_dir)COM (MVS)&quot; | ALL |   |
| cft.cftcom_node.fname | Overrides the CFTCOM by node logical name. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.data_dir)cftcom (not VMS, not MVS) &#124; $(cft.runtime.data_dir)cftcom.rel (VMS) &#124; $(cft.cftcom.fname) (MVS)&quot; | ALL |   |
| cft.cftlog.afname | Replaces the CFTLOGA logical name. | fname | &quot;min length:0, max length:512&quot; | $(cft.cftlog.fname) (not MVS) &#124; $(cft.runtime.log_dir)LOG2 (MVS) | not VMS |   |
| cft.cftlog.afname.atts | Attributes used to create the Log file. | string |   | '$(cft.cftlog.fname.atts)' (MVS) | MVS | EXPERT |
| cft.cftlog.backup_count | Number of backups for Transfer CFT log files. | int |   | 3 | not MVS |   |
| cft.cftlog.exec_timeout | Timeout in seconds for executing a switchlog. | int |   | 10 | not MVS |   |
| cft.cftlog.fname | Overrides the CFTLOG logical name. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.log_dir)cftlog (not VMS, not MVS) &#124; $(cft.runtime.log_dir)cftlog.log (VMS) &#124; $(cft.runtime.log_dir)LOG1 (MVS)&quot; | ALL |   |
| cft.cftlog.fname.atts | Attributes used to create the Log file. | string |   | &quot;'fspace=3000,fblksize=27904' (MVS)&quot; | MVS | EXPERT |
| cft.cftlog.switch_on_start | Enable/disable the LOG switch when CFT start | bool No Yes |   | Yes | not MVS |   |
| cft.cftlog.switch_on_stop | Condition if the log files must be switched at the end of the Transfer CFT stop procedure. | bool No Yes |   | No | ALL | RECONFIG |
| cft.cftlog.time_precision | Define time precision: 1s 10ms 100ms. | int | enum: 1 10 100 | 1 | ALL |   |
| cft.cftparm.fname | Overrides the CFTPARM logical name. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.data_dir)cftparm (not VMS, not MVS) &#124; $(cft.runtime.data_dir)cftparm.inx (VMS) &#124; $(cft.runtime.data_dir)PARM (MVS)&quot; | ALL |   |
| cft.cftparm.keyfname | Transfer CFT key filename. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.conf_dir)cft.key (not MVS) &#124; $(cft.runtime.conf_dir)UPARM(PRODKEY) (MVS) | ALL |   |
| cft.cftparm.partfname | Overrides the CFTPART logical name. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.data_dir)cftpart (not VMS, not MVS, not UNIX, not NT) &#124; $(cft.runtime.data_dir)cftpart.inx (VMS) &#124; $(cft.runtime.data_dir)PART (MVS) &#124; $(cft.runtime.data_dir)cftparm (UNIX, NT)&quot; | ALL |   |
| cft.cftparm.pkifname | Overrides the CFTPKU logical name. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.data_dir)CFTPKU (not MVS, not UNIX, not NT) &#124; $(cft.runtime_dir)PKIFILE (MVS) &#124; $(cft.runtime.data_dir)cftparm (UNIX, NT)&quot; | not VMS |   |
| cft.cftparm.secparm | Overrides the CFTHPARM logical name. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.data_dir)secparm (not VMS) &#124; $(cft.runtime.data_dir)secparm.dat (VMS) | not MVS |   |
| cft.char_directory_protect | &quot;Overrides the default character used to specify a directory. No value means the default character '+', a blank ' ' means no control. Set to another character to override the default one.&quot; | string |   | '' | ALL | EXPERT |
| cft.client.cftcoms.ssl_id | Default identifier of CFTSSL client object used by CFTUTIL to connect to CFTCOMS server if the SSL parameter of the CONFIG command is not specified. | string |   | '' | ALL |   |
| cft.default_idf.enable | Enables the default IDF to use when the transfer request IDF is not explicitly defined by a CFTRECV or CFTSEND command. | bool No Yes |   | Yes | ALL | RECONFIG |
| cft.dirdepth | Lists subdirectory files. | bool Yes No |   | Yes | ALL | RECONFIG |
| cft.fips.enable_compliance | Enables FIPS compliance (Yes/No). | bool Yes No |   | No | &quot;not MVS, not VMS, not HPUX-PARISC-64&quot; |   |
| cft.fips.libcrypto | OpenSSL libcrypto library location. | fname | &quot;min length:0, max length:512&quot; | $(cft.install_dir)/lib/libcrypto.so.1.0.0 (UNIX) &#124; $(cft.install_dir)/lib/libcrypto.a(libcrypto.so.1.0.0) (AIX) &#124; $(cft.install_dir)/lib/libcrypto.sl.1.0.0 (HPUX-PARISC) &#124; $(cft.install_dir)/bin/libeay32.dll (NT) | &quot;not MVS, not VMS, not HPUX-PARISC-64&quot; | EXPERT |
| cft.full_hostname | Full hostname. | string |   | '$(cftfullhostname)' (VMS) | ALL |   |
| cft.guardian.backup_processor | backup processor on which CFT is started (default -1). | int | &quot;min:-1, max:15&quot; | -1 | HP_NONSTOP | EXPERT |
| cft.guardian.cftwrk | Default Guardian pathname for TACL and Netbatch procedures. | string | &quot;min length:0, max length:27&quot; |   | HP_NONSTOP |   |
| cft.guardian.collector | Collector | string |   |   | HP_NONSTOP |   |
| cft.guardian.hometerm | CFT Home Terminal. | string | &quot;min length:2, max length:35&quot; |   | HP_NONSTOP | EXPERT |
| cft.guardian.netbatch.attachment_set | Netbatch attachment set. | string | &quot;min length:1, max length:8&quot; | NBASCFTLI | HP_NONSTOP | EXPERT |
| cft.guardian.netbatch.jobname_prefix | &quot;Prefix for Netbatch JOB Name, Suffix composed of last characters of file created to contain the input TCAL statements.&quot; | string | &quot;min length:0, max length:4&quot; | ZBBT | HP_NONSTOP | EXPERT |
| cft.guardian.netbatch.output | Netbatch output file (default $S.ABTZ). | string | &quot;min length:3, max length:35&quot; | '$S.#ABTZ' | HP_NONSTOP | EXPERT |
| cft.guardian.netbatch.priority | Netbatch Run priority (default -1). | int | &quot;min:-1, max:199&quot; | 90 | HP_NONSTOP | EXPERT |
| cft.guardian.netbatch.process | Netbatch process name. | string | &quot;min length:2, max length:5&quot; | $ZBAT | HP_NONSTOP | EXPERT |
| cft.guardian.netbatch.selpri | Netbatch SELPRI (default 4). | int | &quot;min:1, max:7&quot; | 4 | HP_NONSTOP | EXPERT |
| cft.guardian.nonstop | Enables NonStop mode (Yes/No). | bool Yes No |   | No | HP_NONSTOP |   |
| cft.guardian.priority | CFT priority (default -1). | int | &quot;min:-1, max:199&quot; | -1 | HP_NONSTOP | EXPERT |
| cft.guardian.process_name_prefix | Guardian Process name prefix (2 characters maximum). | string | &quot;min length:0, max length:2&quot; | LA | HP_NONSTOP |   |
| cft.guardian.processor | processor on which CFT is started (default -1). | int | &quot;min:-1, max:15&quot; | -1 | HP_NONSTOP | EXPERT |
| cft.guardian.tacl.backup_processor | backup processor on which TACL is started (default -1). | int |   | -1 | HP_NONSTOP | EXPERT |
| cft.guardian.tacl.hometerm | TACL Home Terminal. | string | &quot;min length:2, max length:35&quot; | $ZHOME | HP_NONSTOP | EXPERT |
| cft.guardian.tacl.output | TACL output file (default $S.ABTT). | string | &quot;min length:3, max length:35&quot; | '$S.#ABTT' | HP_NONSTOP | EXPERT |
| cft.guardian.tacl.priority | TCAL priority (default -1). | int | &quot;min:-1, max:199&quot; | 90 | HP_NONSTOP | EXPERT |
| cft.guardian.tacl.processor | processor on which TACL is started (default -1). | int |   | -1 | HP_NONSTOP | EXPERT |
| cft.guardian.tcpip_resolver_name | TCPIP alternate resolver file name. | string | &quot;min length:0, max length:35&quot; |   | HP_NONSTOP | EXPERT |
| cft.icm.timestamp | Add a timestamp before CFTUTIL messages. | int |   | 0 | ALL |   |
| cft.idparm | Default IDPARM used by Transfer CFT. | string |   | 'IDPARM0' | ALL |   |
| cft.idt.calculation_mode | Sets the calculation method for generating the IDT. Possible values are date (calculation based on the current date) and date_with_leapyear (calculation based on the current date considering that the current year is always a leap year). | string | enum: date date_with_leapyear | 'date_with_leapyear' | ALL |   |
| cft.inner_charsets | List of charsets compatible for BOM removal and space padding. For advanced users only. | string |   | '' | ALL | IRECONFIG EXPERT |
| cft.install.dat_dir | Transfer CFT installation directory. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.install_dir)/distrib/dat/ (not VMS, not MVS) &#124; $(cft.install_dir)distrib.dat] (VMS) &#124; $(cft.install_dir) (MVS)&quot; | ALL | EXPERT |
| cft.install.extrasps_dir | Directory containing the samples used for PassPort. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.install_dir)/extras/PassPort/ (not VMS, not MVS) &#124; $(cft.install_dir)extras.PassPort] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL | EXPERT |
| cft.install.extrassent_dir | Directory containing the samples used for Sentinel. | dir | &quot;min length:0, max length:512&quot; | $(cft.install_dir)/extras/sentinel/ (not VMS) &#124; $(cft.install_dir)extras.sentinel] (VMS) | ALL | EXPERT |
| cft.install.psenglish_dir | Directory containing the messages used for PassPort. | dir | &quot;min length:0, max length:512&quot; | $(cft.install_dir)/distrib/ps/english/ (not VMS) &#124; $(cft.install_dir)distrib.ps.english] (VMS) | ALL | EXPERT |
| cft.install.tfenglish_dir | Directory containing the messages used for TrustedFile. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.install_dir)/distrib/tf/english/ (not VMS, not MVS) &#124; $(cft.install_dir)distrib.tf.english] (VMS) &#124; $(cft.runtime_dir)PKIMSG (MVS)&quot; | ALL | EXPERT |
| cft.install.xsr_dir | Transfer CFT Secure Relay installation directory. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.install_dir)/distrib/xsr/ (not MVS, not VMS) &#124; $(cft.install_dir)distrib.xsr] (VMS)&quot; | ALL | EXPERT |
| cft.install_dir | Transfer CFT installation directory. | dir | &quot;min length:0, max length:512&quot; | $(CFTDIRINSTALL) | ALL |   |
| cft.instance_group | Transfer CFT instance GROUP. | string |   | 'Production.VMS' (VMS) &#124; 'Production.IBMi' (OS400) | ALL |   |
| cft.instance_id | Transfer CFT instance ID. | string |   | 'CFT_$(cft$user)' (VMS) | ALL |   |
| cft.instance_uid | Transfer CFT instance unique ID. | string |   | '' | ALL |   |
| cft.ipv6.disable_connect | Disables IPv6 connections. | bool Yes No |   | Yes | ALL |   |
| cft.ipv6.disable_listen | Disables IPv6 for listening. | bool Yes No |   | Yes | ALL |   |
| cft.ipv6.set_ai_numerichost | Sets AI_NUMERICHOST hint before getaddrinfo() call. Use to prevent API system getaddrinfo from performing unnecessary DNS requests for numeric hostnames. | bool Yes No |   | Yes | ALL |   |
| cft.ipv6.set_ai_numericserv | Sets AI_NUMERICSERV hint before getaddrinfo() call. Use to prevent API system getaddrinfo from performing unnecessary DNS requests for numeric hostnames. | bool Yes No |   | Yes | ALL |   |
| cft.ipv6.use_ipv4_legacy_resolver | &quot;Use legacy IPv4 only host and service names resolution API, that is, gethostbyname() and getservbyname().&quot; | bool Yes No |   | No | ALL |   |
| cft.jre.java_binary_path | Java binary path used to start Transfer CFT jar files. | fname | &quot;min length:0, max length:512&quot; |   | &quot;NT, UNIX&quot; | EXPERT |
| cft.jre.loader | Loads the JRE (Java Runtime Environment). | fname | &quot;min length:0, max length:512&quot; | $(cft.install_dir)/distrib/java/CFTJRE.jar | &quot;NT, UNIX&quot; | EXPERT |
| cft.jre.pid_fname | File containing the JRE Process ID. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)jre.pid | &quot;NT, UNIX, VMS&quot; | EXPERT |
| cft.jre.run.pid | JRE Process ID. | string |   | '1' | &quot;NT, UNIX&quot; | RUNTIME |
| cft.jre.start_options | JRE start options. | string |   | &quot;'-Xmx1024m -Dlog4j.configuration=conf/cft-log4j.properties -Dhttp.strictPostRedirect=true -Djdk.http.auth.tunneling.disabledSchemes=&quot;&quot;&quot;&quot;' (not NT) &#124; '-Xmx512m -Xrs -Dstdout_fname=run/cftjre.out -Dlog4j.configuration=conf/cft-log4j.properties -Dhttp.strictPostRedirect=true -Djdk.http.auth.tunneling.disabledSchemes=&quot;&quot;&quot;&quot;' (NT)&quot; | &quot;NT, UNIX&quot; | EXPERT |
| cft.listcat_compat | Condition if the default listcat format does or doesn't include phase and phasestep. | bool No Yes |   | No | ALL |   |
| cft.multi_node.cftcom.dispatcher_policy | Used to dispatch transfers with HOLD state. | string | enum: round_robin node_affinity | 'round_robin' | ALL | EXPERT |
| cft.multi_node.cftcom.lock_fname | Lock file for CFTCOM task in multi-node. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)cftcom.lck | not MVS | EXPERT |
| cft.multi_node.cftcom.lock_retry_delay | Delay in seconds for CFTMCOM (multinode master) lock retry delay. | int |   | $(cft.multi_node.lock_retry_delay) | ALL | EXPERT |
| cft.multi_node.cftcron.lock_fname | Lock file for cronjobs scheduler task in multi-node. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)cftcron.lck | ALL | EXPERT |
| cft.multi_node.cftcron.lock_retry_delay | Delay in seconds for cftcron lock retry. | int |   | $(cft.multi_node.lock_retry_delay) | ALL | EXPERT |
| cft.multi_node.cftdscan.lock_fname | Lock file for CFTDSCAN task in multi-node. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime_dir)/run/cftdscan.lck (not OS400) &#124; *LIBL/DSCLCK (OS400) &#124; $(cft.runtime_dir)run]cftdscan.lck (VMS) | not MVS | EXPERT |
| cft.multi_node.connection_retry_delay | Delay in seconds for connection retry. | int |   | 10 | ALL | EXPERT |
| cft.multi_node.enable | Enables the multi-node architecture. | bool Yes No |   | No | ALL |   |
| cft.multi_node.enable_spurious_log_messages | &quot;If Yes, shows all log messages for all nodes, even if it is not relevant for each one&quot; | bool Yes No |   | No | ALL | EXPERT |
| cft.multi_node.hostnames | List of hosts that handle the multi-node architecture. | string list |   |   | ALL | EXPERT |
| cft.multi_node.hostnames.&lt;logical_name&gt;.control_port | Control port of (copsmng) in multi-node. | int |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.copcod_local_port | Connection dispatcher local port in multi-node. | int |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.copui_client_socket | Windows socket passing for UI server (copui) in multi-node. | int |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.copui_client_ssl_socket | Windows ssl socket passing for UI server (copui) in multi-node. | int |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.copui_notification_port | Notification port for UI server (copui) in multi-node. | int |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.copui_pid | copui Process ID in multi-node. | string |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.host | Address (FQDN or IP address) of the host. | string |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.node_manager_local_port | Node manager local port of (copnman) in multi-node. | int |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.node_manager_port | Node manager port of (copnman) in multi-node. | int |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.pid | Copilot Process ID (copsmng) in multi-node. | string |   |   | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.state | Copilot (copsmng) status in multi-node. | string | enum: INITIALIZING STARTING RUNNING STOPPING STOPPED ERROR | STOPPED | ALL |   |
| cft.multi_node.hostnames.&lt;logical_name&gt;.state_timestamp | Timestamp of the last Copilot status update. | string |   |   | ALL |   |
| cft.multi_node.listen_port_range | &quot;Defines a port range to use for listening points dedicated to inter-node communication in multi-node. The syntax is startingPort-EndingPort. For example '33000-33100'. If empty, the default Operating System port numbers range is used.&quot; | string |   | '' | ALL |   |
| cft.multi_node.load_balancer.host | Load balancer address (FQDN or IP address) used by Central Governance to connect to Transfer CFT UI Server. | string |   | '' | ALL |   |
| cft.multi_node.load_balancer.port | Load balancer port used by Central Governance to connect to Transfer CFT UI Server port dedicated to Central Governance (default 1767). | int |   |   | ALL |   |
| cft.multi_node.lock_retry_delay | Delay in seconds for cftcron and CFTMCOM (multinode master) lock retry. | int |   | 20 | ALL | EXPERT |
| cft.multi_node.max | Maximum number of nodes. | int |   | 8 | ALL | EXPERT |
| cft.multi_node.max_per_host | Maximum number of nodes (CFTMAIN) per host. | int |   | $(cft.multi_node.max) | ALL | EXPERT |
| cft.multi_node.nodes | Number of nodes (from 2 to max_nodes) | int |   | 2 | ALL | EXPERT |
| cft.multi_node.nodes.&lt;itemNumber&gt;.coms_port | Internal listening port of COMS server for a node. | int |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.configuration_version | Stamp of the configuration in which Transfer CFT has been started. | string |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.disabling | Flag set when CFT is disabling. | bool Yes No |   | No | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.host | Host address of the server where the node is running on. | string |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.hostname | Hostname of the server where the node is running on. | string |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.jre_pid | JRE Process ID. | int |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.msg | Message describing the current startup or shutdown process. | string |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.nodestate | Node status. | string | enum: DISABLED ENABLED_STOPPED ENABLED_STARTED | DISABLED | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.pid | CFTMAIN Process ID in multi-node. | string |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.progress | Indicates the startup or shutdown progress expressed as a percentage. | int |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.prx_port | Internal listening port of a node. | int |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.starting | Flag set when CFT is starting. | bool Yes No |   | No | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.state | Transfer CFT status. | string | enum: INITIALIZING STARTING RUNNING STOPPING STOPPED ERROR | STOPPED | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.state_timestamp | Timestamp of the last Transfer CFT status update. | string |   |   | ALL |   |
| cft.multi_node.nodes.&lt;itemNumber&gt;.stopping | Flag set when CFT is stopping. | bool Yes No |   | No | ALL |   |
| cft.multi_node.policy_fname | &quot;*(*) full name, *(#) node id&quot; | string  |   | '*(*).N*(#)' (MVS) | ALL | EXPERT |
| cft.multi_node.registration.lock_fname | Lock file for registration task to Central Governance in multi-node. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)regist.lck | ALL | EXPERT |
| cft.multi_node.shared.filesystem.type | Used to select appropriate consistency enforcement strategy. | string | enum: unknown posix nfs cifs | 'unknown' | ALL | EXPERT |
| cft.multi_node.sharedidt.enable | Use global IDT calculation method. | bool Yes No |   | No | ALL | EXPERT |
| cft.multi_node.sharedidt.fname | Shared file for global IDT calculation in multi-node. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.data_dir)cftsidt | &quot;NT, UNIX, MVS, OS400&quot; | EXPERT |
| cft.multi_node.start_node.proc_fname | Procedure to start Transfer CFT nodes. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime_dir)INSTALL(MNRMON) | MVS | EXPERT |
| cft.multi_node.start_node.user | Userid used to start Transfer CFT nodes (lib APF). | string |   | ' ' | MVS | EXPERT |
| cft.multi_node.transfer_recovery_retry_delay | Delay in seconds for transfer recovery retry. | int |   | 20 | ALL | EXPERT |
| cft.multi_node.transfer_recovery_timeout | Timeout in seconds for the transfer recovery process. | int |   | 30 | ALL | EXPERT |
| cft.mvs.check_exec | &quot;Control procedures submitted Yes:'/', Value:'//* EXEC CFT'.&quot; | string | enum: no yes value | 'no' | MVS |   |
| cft.mvs.copilot.check_apf | YES indicates Copilot server must be APF-authorized to start. | bool Yes No |   | No | MVS | EXPERT |
| cft.mvs.monitor.check_apf | YES indicates Transfer CFT must be APF-authorized to start. | bool Yes No |   | No | MVS | EXPERT |
| cft.mvs.multi_node.monitor.arm | Forces REGISTER to ARM in multi-node (if ARM=YES in SGINSTAL). | bool Yes No |   | No | MVS | EXPERT |
| cft.mvs.sginstal.allnext | The % factor applied to the secondary allocation when creating a multi-volume file. | int | &quot;min:1, max:255&quot; | 100 | MVS | EXPERT |
| cft.mvs.sginstal.allone | The % factor applied to the primary allocation computed to create a multi-volume file. | int | &quot;min:1, max:255&quot; | 100 | MVS | EXPERT |
| cft.mvs.sginstal.allprim | The % factor applied to the primary allocation computed to create a single volume file. | int | &quot;min:1, max:255&quot; | 100 | MVS | EXPERT |
| cft.mvs.sginstal.allsec | The % of the primary space used as the secondary allocation when creating a single volume file. | int | &quot;min:1, max:255&quot; | 10 | MVS | EXPERT |
| cft.mvs.sginstal.arm | Support of the Automatic Restart Manager. | bool Yes No |   | Yes | MVS | EXPERT |
| cft.mvs.sginstal.blkpds | Number of blocks allocated while creating a partitioned file. | int | &quot;min:1, max:32760&quot; | 150 | MVS | EXPERT |
| cft.mvs.sginstal.blksize | Default BLKSIZE. | int | &quot;min:4100, max:32760&quot; | 27920 | MVS | EXPERT |
| cft.mvs.sginstal.desc | Value of the Descriptor code field of the WTO. | string |   | '0x0000' | MVS | EXPERT |
| cft.mvs.sginstal.dsntype | Value added to create DF/SMS managed EXTENTED or LARGE files. | string | enum: none extpref extreq large | 'none' | MVS | EXPERT |
| cft.mvs.sginstal.emcsopt | Option to manage MODIFY command. | string | enum: user monitor ignore | 'user' | MVS | EXPERT |
| cft.mvs.sginstal.formatvb | Default RECFM (VB). | bool Yes No |   | No | MVS | EXPERT |
| cft.mvs.sginstal.hsmasync | Option to manage synchronously HSM recall. | bool Yes No |   | No | MVS | EXPERT |
| cft.mvs.sginstal.m64tdef | 64 b memory for default task (in MB). | int |   | 16 | MVS | EXPERT |
| cft.mvs.sginstal.m64tfil | 64 b memory for task TFIL (in MB). | int |   | 3 | MVS | EXPERT |
| cft.mvs.sginstal.m64tlog | 64 b memory for TLOG task (in MB). | int |   | 64 | MVS | EXPERT |
| cft.mvs.sginstal.m64tmai | 64 b memory for main task (in MB). | int |   | 64 | MVS | EXPERT |
| cft.mvs.sginstal.m64tpro | 64 b memory for task TPRO (in MB). | int |   | 64 | MVS | EXPERT |
| cft.mvs.sginstal.m64tssl | 64 b memory for task TSSL (in MB). | int |   | 64 | MVS | EXPERT |
| cft.mvs.sginstal.m64ttrk | 64 b memory for task TTRK (in MB). | int |   | 256 | MVS | EXPERT |
| cft.mvs.sginstal.maxab | Number of ABENDs allowed by Transfer CFT before shutting done. | int | &quot;min:1, max:255&quot; | 15 | MVS | EXPERT |
| cft.mvs.sginstal.maxdump | Number of ABENDs that are th object of a Transfer CFT requested DUMP. | int | &quot;min:1, max:255&quot; | 2 | MVS | EXPERT |
| cft.mvs.sginstal.maxtrace | Limit size for SGTRACE. | int |   | 80000 | MVS | EXPERT |
| cft.mvs.sginstal.mcsopt | Option to manage MODIFY command. | string | enum: check monitor | 'check' | MVS | EXPERT |
| cft.mvs.sginstal.pdsesharing | Yes allow simultaneous writing to a PDSE file type. | bool Yes No |   | No | MVS | EXPERT |
| cft.mvs.sginstal.routcde | Value of the ROUTCODE field of the WTO. | string |   | '0x0008' | MVS | EXPERT |
| cft.mvs.sginstal.sdsfopt | Option to manage MODIFY command. | string | enum: user monitor ignore | 'user' | MVS | EXPERT |
| cft.mvs.sginstal.sgtrace | Initial value of the SGTRACE trace file. | int | &quot;min:0, max:65535&quot; | 0 | MVS | EXPERT |
| cft.mvs.sginstal.sharecat | Cache catalog option. | string | enum: yes no inact | 'yes' | MVS | EXPERT |
| cft.mvs.sginstal.subopt | Option to manage statistic lines for a job submitted. | int | enum: 0 1 3 | 0 | MVS | EXPERT |
| cft.mvs.sginstal.tape | Option to manage support tape files. | string | enum: notsup update output | 'UPDATE' | MVS | EXPERT |
| cft.mvs.sginstal.trace | Transfer CFT internal trace size (in KB). | int | &quot;min:4, max:16383&quot; | 128 | MVS | EXPERT |
| cft.mvs.sginstal.tsoedit | File support with sequence number in columns 73 to 80. | bool Yes No |   | No | MVS | EXPERT |
| cft.mvs.sginstal.volnum | The maximum number of volumes in a multi-volume allocation. | int | &quot;min:1, max:127&quot; | 20 | MVS | EXPERT |
| cft.mvs.sginstal.vsamsufs | &quot;Suffix for the VSAM (ESDS, KSDS) DATA and INDEX components. (SHORT for .D and .I (default), LONG for .DATA and .INDEX).&quot; | string | enum: SHORT LONG | 'SHORT' | MVS | EXPERT |
| cft.nt.cftw_display_log_messages | Display CFTLOG instead of CFTMAIN in CFTW. | bool Yes No |   | Yes | NT |   |
| cft.nt.cftw_trcfilename | CFTW trace. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)cftw.trc | NT |   |
| cft.nt.service_mode | Transfer CFT is installed in service mode | bool Yes No |   | No | NT |   |
| cft.nt.service_name | Transfer CFT service name in Windows. | string |   | CFTService | NT |   |
| cft.nt.start_graphmode | Start Transfer CFT either in graphical mode or not. | bool Yes No |   | Yes | NT |   |
| cft.nt.start_timeout | &quot;Sets the Transfer CFT time limit to start up. If the Transfer CFT status does not change within the delay, it is assumed that the 'cft start' command failed.&quot; | int |   | 30 | NT |   |
| cft.nt.stop_timeout | &quot;Sets the Transfer CFT time limit to stop. If the Transfer CFT status does not change within the delay, it is assumed that the stop failed.&quot; | int |   | 30 | NT |   |
| cft.output.backup_count | Number of backups for the output file. | int |   | 3 | not MVS |   |
| cft.output.backup_time | Time at which the automatic backup is performed. The time format is HHMMSS. | time |   | 100 | ALL |   |
| cft.output.fname | Output file. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)cft.out (not MVS) &#124; (MVS) | ALL | EXPERT |
| cft.probes.history_size | Number of probe items in the history for each time class. | int |   | 20 | ALL | RECONFIG |
| cft.probes.periodicity | Time interval between the probe samples in seconds. | int |   | 60 | ALL | RECONFIG |
| cft.probes.time_classes | &quot;List of history time classes: the default is 1 second, 10 seconds, 1 minute, 10 minutes...&quot; | string |   | 1 10 60 600 3600 21600 86400 604800 | ALL | RECONFIG |
| cft.purge.background_on_start | Defines if the start-up purge is run in background mode. | bool Yes No |   | Yes | ALL |   |
| cft.purge.enable_on_start | Defines if the purge must be run at start-up. | bool Yes No |   | Yes | ALL |   |
| cft.purge.periodicity | &quot;Time interval in days (x or xD), in hours (xH) or in minutes (xM) between two automatic purges of entries in the transfers catalog.&quot; | string |   | '0' | ALL | RECONFIG |
| cft.purge.quantity | Number of transfers to delete in one step. | int |   | 10 | ALL | RECONFIG |
| cft.purge.rh | &quot;The amount of time in days (x or xD), in hours (xH) or in minutes (xM) to keep HOLD receiver transfers before purging. If not set to -1, overrides the RH parameter of the CFTCAT object.&quot; | string |   | '-1' | ALL | RECONFIG |
| cft.purge.rt | &quot;The amount of time in days (x or xD), in hours (xH) or in minutes (xM) to keep TERMINATED receiver transfers before purging. If not set to -1, overrides the RT parameter of the CFTCAT object.&quot; | string |   | '-1' | ALL | RECONFIG |
| cft.purge.rx | &quot;The amount of time in days (x or xD), in hours (xH) or in minutes (xM) to keep DONE receiver transfers before purging. If not set to -1, overrides the RX parameter of the CFTCAT object.&quot; | string |   | '-1' | ALL | RECONFIG |
| cft.purge.ry | &quot;The amount of time in days (x or xD), in hours (xH) or in minutes (xM) to keep POSTPROCESSING receiver transfers before purging. If not set to -1, overrides the RY parameter of the CFTCAT object.&quot; | string |   | '-1' | ALL | RECONFIG |
| cft.purge.sh | &quot;The amount of time in days (x or xD), in hours (xH) or in minutes (xM) to keep HOLD sender transfers before purging. If not set to -1, overrides the SH parameter of the CFTCAT object.&quot; | string |   | '-1' | ALL | RECONFIG |
| cft.purge.st | &quot;The amount of time in days (x or xD), in hours (xH) or in minutes (xM) to keep TERMINATED sender transfers before purging. If not set to -1, overrides the ST parameter of the CFTCAT object.&quot; | string |   | '-1' | ALL | RECONFIG |
| cft.purge.sx | &quot;The amount of time in days (x or xD), in hours (xH) or in minutes (xM) to keep DONE sender transfers before purging. If not set to -1, overrides the SX parameter of the CFTCAT object.&quot; | string |   | '-1' | ALL | RECONFIG |
| cft.purge.sy | &quot;The amount of time in days (x or xD), in hours (xH) or in minutes (xM) to keep POSTPROCESSING sender transfers before purging. If not set to -1, overrides the SY parameter of the CFTCAT object.&quot; | string |   | '-1' | ALL | RECONFIG |
| cft.readline.enable | Enables readline history for CFTUTIL | bool Yes No |   | Yes | &quot;UNIX, NT&quot; |   |
| cft.readline.history_fname | Readline history file. | fname | &quot;min length:0, max length:512&quot; | $(HOME)/.cft_history (UNIX) &#124; %APPDATA%\cft\CftutilHistory.txt (NT) | &quot;UNIX, NT&quot; |   |
| cft.readline.history_size | Readline history size. | int |   | 500 | &quot;UNIX, NT&quot; |   |
| cft.readline.pkihistory_fname | Readline pki history file. | fname | &quot;min length:0, max length:512&quot; | $(HOME)/.cft_pkihistory (UNIX) &#124; %APPDATA%\cft\CftPkiUtilHistory.txt (NT) | &quot;UNIX, NT&quot; |   |
| cft.run.configuration_version | Stamp of the configuration in which Transfer CFT has been started. | string |   | ' ' | ALL | RUNTIME |
| cft.run.idparm | Last IDPARM used by Transfer CFT. | string |   | 'IDPARM0' | ALL | RUNTIME |
| cft.run.maxtrans | Last maxtrans used by Transfer CFT. | int |   | 0 | ALL | RUNTIME |
| cft.run.msg | Message describing the current startup or shutdown process. | string |   | '' | ALL | RUNTIME |
| cft.run.pid | CFTMAIN Process ID. | string |   | '1' | ALL | RUNTIME |
| cft.run.pid_fname | File containing the CFTMAIN Process ID. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.run_dir)cft.pid (not MVS, not OS400)&quot; | &quot;NT, UNIX, VMS, OS400&quot; | EXPERT |
| cft.run.progress | Indicates the startup or shutdown progress expressed as a percentage. | int |   | 0 | ALL | RUNTIME |
| cft.run.state | Transfer CFT status. | string | enum: INITIALIZING STARTING RUNNING STOPPING STOPPED ERROR | 'STOPPED' | ALL | RUNTIME |
| cft.run.state_timestamp | Timestamp of the last Transfer CFT status update. | string |   | ' ' | ALL | RUNTIME |
| cft.runtime.accnt_dir | Directory containing the accounting files. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/accnt/ (not VMS, not MVS) &#124; $(cft.runtime_dir)accnt] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL |   |
| cft.runtime.conf_dir | Directory containing the sample configuration files. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/conf/ (not VMS, not MVS) &#124; $(cft.runtime_dir)conf] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL |   |
| cft.runtime.confpki_dir | Directory containing the certificates. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/conf/pki/ (not VMS, not MVS) &#124; $(cft.runtime_dir)conf.pki] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL |   |
| cft.runtime.conftf_dir | Directory containing the sample TrustedFile configuration. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/conf/tf/ (not VMS, not MVS) &#124; $(cft.runtime_dir)conf.tf] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL |   |
| cft.runtime.crypto_dir | Directory containing the Transfer CFT cryptographic data. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/data/crypto/ (not VMS, not MVS) &#124; $(cft.runtime_dir)data.crypto] (VMS) &#124; $(cft.runtime_dir)CRYPTO (MVS)&quot; | ALL |   |
| cft.runtime.data_dir | Directory containing the Transfer CFT databases. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/data/ (not VMS, not MVS) &#124; $(cft.runtime_dir)data] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL |   |
| cft.runtime.datatf_dir | Working directory for TrustedFile. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/data/tf/ (not VMS, not MVS) &#124; $(cft.runtime_dir)data.tf] (VMS) &#124; $(cft.runtime_dir)TF. (MVS)&quot; | ALL |   |
| cft.runtime.log_dir | Directory containing the log files. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/log/ (not VMS, not MVS) &#124; $(cft.runtime_dir)log] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL |   |
| cft.runtime.run_dir | Directory containing the runtime files. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/run/ (not VMS, not MVS) &#124; $(cft.runtime_dir)run] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL |   |
| cft.runtime.traces_dir | Directory containing the traces files. | dir | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/traces/ (not VMS, not MVS) &#124; $(cft.runtime_dir)traces] (VMS) &#124; $(cft.runtime_dir) (MVS)&quot; | ALL |   |
| cft.runtime.xsr_dir | Transfer CFT Secure Relay runtime directory. | dir | &quot;min length:0, max length:512&quot; |   | MVS | EXPERT |
| cft.runtime_dir | Transfer CFT runtime directory. | dir | &quot;min length:0, max length:512&quot; | $(CFTDIRRUNTIME) | ALL |   |
| cft.sched.partner_policy | &quot;Defines scheduling partner policy, either round robin or FIFO.&quot; | string |   | 'round_robin' | ALL | EXPERT RECONFIG |
| cft.scheduled_values | List of scheduled aliases. Allows user to schedule a temporary value of a specified parameter. | string list |   |   | ALL |   |
| cft.scheduled_values.&lt;logical_name&gt;.delay | Delay using the format 'MM:HH'. This is the length of time during which the value can be changed. | string |   | 0:00 | ALL |   |
| cft.scheduled_values.&lt;logical_name&gt;.id | The configuration entity id (uconf parameter) that you want to provide scheduling for. | string |   |   | ALL |   |
| cft.scheduled_values.&lt;logical_name&gt;.start_time | Start time using the format 'MM:HH:DAYS_OF_THE_WEEK'. This is the begin time for when a value switches from its existing value to the temporary value | string |   | 00:00:* | ALL |   |
| cft.scheduled_values.&lt;logical_name&gt;.value | Temporary value. This value overrides the existing configured value of the defined uconf parameter. | string |   |   | ALL |   |
| cft.secure_open_mode | Refuse transfer in open mode when fname contains '\..\' or '/../' after substitution. | bool No Yes |   | No | ALL | EXPERT RECONFIG |
| cft.seed.enable_internal | Enables internal randomized for SSL SEED. | bool Yes No |   | No (not OS400) &#124; Yes (OS400) | not MVS | EXPERT |
| cft.server.authentication_method | Server Authentication method (used by COMS and RPASSWD/SPASSWD). | string |   | 'none' | ALL |   |
| cft.server.bandwidth.cos | Number of class-of-service. | int |   | 1 | ALL | RECONFIG |
| cft.server.bandwidth.cos.0 | #ERROR | #ERROR |   |   | ALL |   |
| cft.server.bandwidth.cos.&lt;itemNumber&gt;.comment | User comment that describes the class-of-service. | string |   |   | ALL |   |
| cft.server.bandwidth.cos.&lt;itemNumber&gt;.max_rate_in | Maximum rate in KBytes of incoming data for that class-of-service. -1 means unlimited. | int |   | -1 | ALL |   |
| cft.server.bandwidth.cos.&lt;itemNumber&gt;.max_rate_out | Maximum rate in KBytes of outgoing data for that class-of-service. -1 means unlimited. | int |   | -1 | ALL |   |
| cft.server.bandwidth.cos.&lt;itemNumber&gt;.parent | Parent class-of-service. | int |   | 0 | ALL |   |
| cft.server.bandwidth.cos.&lt;itemNumber&gt;.session_max_rate_in | Maximum rate in KBytes of incoming data for sessions under that class-of-service. -1 means unlimited. | int |   | -1 | ALL |   |
| cft.server.bandwidth.cos.&lt;itemNumber&gt;.session_max_rate_out | Maximum rate in KBytes of outgoing data for sessions under that class-of-service. -1 means unlimited. | int |   | -1 | ALL |   |
| cft.server.bandwidth.cos.&lt;itemNumber&gt;.weight_in | Use to compute the nominal rate of incoming data for this class of service. -1 means unlimited. | int |   | -1 | ALL |   |
| cft.server.bandwidth.cos.&lt;itemNumber&gt;.weight_out | Use to compute the nominal rate of outgoing data for this class of service. -1 means unlimited. | int |   | -1 | ALL |   |
| cft.server.bandwidth.cos_requester_default | Default class-of-service for requester mode transfers. | int |   | 0 | ALL | RECONFIG |
| cft.server.bandwidth.cos_server_default | Default class-of-service for server mode transfers. | int |   | 0 | ALL | RECONFIG |
| cft.server.bandwidth.delay | Bandwidth control granularity (in microsecond) : the amount of time between refreshing token bucket. | int |   | 1000000 | ALL | EXPERT RECONFIG |
| cft.server.bandwidth.enable | Enables the network bandwidth used for incoming and outgoing data in flows. | bool No Yes |   | No | ALL | RECONFIG |
| cft.server.bandwidth.enable_borrowing | Enables bandwidth borrowing (passive borrowing). | bool Yes No |   | Yes | ALL | EXPERT RECONFIG |
| cft.server.bandwidth.smoothing_factor | &quot;The bandwidth smoothing factor optimizes resources to flatten, or smooth, bandwidth usage. Values range from 1 to 10, where 1 is the smoothest and 10 may result in irregular bandwidth usage.&quot; | int |   | 3 | ALL | EXPERT RECONFIG |
| cft.server.catalog.cache_size | Size of the catalog cache. | int |   | 1000 | ALL | EXPERT RECONFIG |
| cft.server.catalog.sync.enable | Enables sync at each catalog flush. | bool No Yes |   | No | ALL | EXPERT RECONFIG |
| cft.server.cftcoms.authentication_enable | Enables synchronous communication authentication. | bool No Yes |   | No | ALL |   |
| cft.server.cftcoms.max_connection | The maximum number of connections. | int |   | 256 | ALL |   |
| cft.server.com.multinode_watchperiod | Interval in seconds between two checks of Transfer CFT nodes status. | int |   | 50 | ALL |   |
| cft.server.com.rescan.enable | Enables file communication rescan. | bool No Yes |   | No | ALL | EXPERT RECONFIG |
| cft.server.com.rescan.wscan_factor | &quot;If rescan is enabled, rescan the entire COM file every WSCAN times this factor.&quot; | int |   | 50 | ALL | EXPERT RECONFIG |
| cft.server.delayed_update | Performance parameter where the delay is the number of seconds before next catalog flush. | int |   | 1 | ALL | EXPERT RECONFIG |
| cft.server.event_queue_length | Length of the internal events queue. The maximum number of events handled in one internal iteration. | int |   | 10 | ALL | EXPERT RECONFIG |
| cft.server.exec_as_user | &quot;Execute transfer processing (pre, post, and ack processing) on the behalf of the user (CFTSEND/CFTRECV).&quot; | bool No Yes |   | &quot;No (NT, UNIX) &#124; Yes (MVS)&quot; | &quot;NT, UNIX, MVS&quot; |   |
| cft.server.fil.main_loop_delay | &quot;If this value is not 0, add delay in main loop in microseconds.&quot; | int |   | 0 | ALL | EXPERT RECONFIG |
| cft.server.force_heterogeneous_mode | Force heterogeneous mode for group file transfers. Replace the deprecated environment variable: CFTSFMCPY. | bool No Yes |   | No | &quot;NT, UNIX&quot; |   |
| cft.server.log.exclude_filters | List of logical identifiers. Allows user to exclude messages from the CFTLOG according to matching filters. | string list |   | filter1 | ALL |   |
| cft.server.log.exclude_filters.&lt;logical_name&gt;.comment | Free comment. | string |   |   | ALL |   |
| cft.server.log.exclude_filters.&lt;logical_name&gt;.pattern | Pattern of the log message to exclude. The wildcard '*' can be used for the pattern. | string |   |   | ALL |   |
| cft.server.max_processing_scripts | &quot;Set to a non-zero integer to define the maximum number of processing scripts, other than EXECE, that can be executed in parallel. Zero (0) disables the feature.&quot; | int |   | 0 | ALL |   |
| cft.server.max_session | Maximum number of simultaneous sessions. | int |   | 0 | ALL |   |
| cft.server.maxtrans | Overrides the maxtrans of CFTPARM object (The maximum authorized number of transfers in parallel) parameter. | int |   | 0 (not UNIX) &#124; $(cft.unix.active_trans) (UNIX) | ALL |   |
| cft.server.nrdp.conn_retry_delay_max | Retry delay max in seconds when network resource depletion is detected. | int | max:60 | 30 | ALL |   |
| cft.server.nrdp.conn_retry_delay_min | Retry delay min in seconds when network resource depletion is detected. | int | min:2 | 5 | ALL |   |
| cft.server.nrdp.enable | Enable prevention of network resource depletion. | bool No Yes |   | No | ALL |   |
| cft.server.parent_transfer.error_management_policy | Error management policy for parent transfer request. IMMEDIATE: The parent transfer error occurs immediately if a child transfer is in error. PHASECOMPLETED: The parent transfer error occurs only after the current phase is completed. | string | enum: IMMEDIATE PHASECOMPLETED | 'IMMEDIATE' | ALL |   |
| cft.server.parm.cache_size | Cache size in number of entries for the PARM and PART databases. The zero value disables the cache. | int | &quot;min:0, max:10000&quot; | 5000 | ALL |   |
| cft.server.parm.cache_timeout | &quot;Timeout in seconds, that specifies the PARM/PART cache expiration. The zero value disables the expiration.&quot; | int | &quot;min:0, max:86400&quot; | 60 | ALL |   |
| cft.server.processing_scripts_variables_blacklist | &quot;POSIX Regular Extended expression that defines forbidden characters. These characters should not be used in symbolic variables, \&quot; | string |   | '`&#124;\$\(&#124;;&#124;&amp;&#124;\&#124;' (UNIX) &#124; '&amp;' (NT) | &quot;UNIX, NT&quot; |   |
| cft.server.read_buffer_size | &quot;Read buffer size for transferred sequential files (-1: Auto, 0: no buffer, n: buffer size).&quot; | int |   | -1 | &quot;NT, UNIX, OS400&quot; | EXPERT IRECONFIG |
| cft.server.send_abort_on_shut | Sends protocol ABORT on CFT shut if set to Yes. | bool Yes No |   | No | ALL | EXPERT |
| cft.server.tcp.max_count_to_log_incoming_silent_connections | The maximum number of incoming silent connections to reach before displaying a message in the log. Enter 0 to disable. | int |   | 0 | ALL | RECONFIG |
| cft.server.tcp.stat.log.interval | Interval in seconds between LOG of TCP statistics and counters. | int |   | 0 | ALL | EXPERT |
| cft.server.transfer.max_files_listed | Maximum number of files when sending a group of files in heterogeneous mode or when listing the contents of a folder. A zero value disables the limit. | int | min:0 | 100000 | UNIX |   |
| cft.server.transfer.raise_error_when_exec_not_found | &quot;Raise an error when the defined procedure script is not found (i.e. the PHASESTEP is set to K, with DIAGI=155 and DIAGP=NO EXEC). This parameter is applicable to post-processing (EXEC and EXECE) and ack-processing (ACKEXEC).&quot; | bool No Yes |   | Yes | ALL |   |
| cft.server.transfer.rrename.max_retries | Maximum number of retries. | int | &quot;min:1, max:65535&quot; | 10 | ALL |   |
| cft.server.transfer.rrename.retry_delay | Delay in seconds between two retries for rename. | int | &quot;min:1, max:65535&quot; | 60 | ALL |   |
| cft.server.transfer.tcp.listen_backlog | Sets the TCP listen backlog size (value 0 means SO_MAXCONN). | int |   | 0 (not NT) &#124; 20 (NT) | ALL |   |
| cft.server.write_buffer_size | &quot;Write buffer size for transferred sequential files (-1: Auto, 0: no buffer, n: buffer size).&quot; | int |   | -1 | &quot;NT, UNIX, OS400&quot; | EXPERT IRECONFIG |
| cft.short_hostname | Hostname. | string |   | '$(UCX$INET_HOST)' (VMS) | ALL |   |
| cft.ssl.version_max | Maximum SSL version allowed by Transfer CFT monitor for business flows. | string | enum: ssl_3.0 tls_1.0 tls_1.1 tls_1.2 | 'tls_1.2' | ALL |   |
| cft.ssl.version_min | Minimum SSL version allowed by Transfer CFT monitor for business flows. | string | enum: ssl_3.0 tls_1.0 tls_1.1 tls_1.2 | '$(ssl.version_min)' | ALL |   |
| cft.state_compat | &quot;Defines if backward compatibility is enabled, with either the use of phase and phasestep or the formerly used.&quot; | bool No Yes |   | No | ALL |   |
| cft.support_dir | Directory where cft-support archives are stored. | dir | &quot;min length:0, max length:512&quot; | $(CFTDIRRUNTIME)/cft-support | &quot;UNIX, NT&quot; |   |
| cft.synchrony_dir | Synchrony installation directory. | dir | &quot;min length:0, max length:512&quot; |   | not MVS |   |
| cft.uconf.cftext | Condition if uconf configuration is extracted using CFTUTIL CFTEXT TYPE=ALL. | bool No Yes |   | Yes | ALL |   |
| cft.uconf.default_fname | Path to uconf (unified configuration) default database. | fname | &quot;min length:0, max length:512&quot; | $(cft.install.dat_dir)cftuconf-common.dat (not MVS) &#124; DD:DEFAULT (MVS) | ALL | EXPERT |
| cft.uconf.dictionary_version | Version of the UCONF dictionary file. | string |   | '[CFT_VERSION] SP[CFT_SP]' | ALL | READ_ONLY |
| cft.uconf.instance_fname | Path to uconf (unified configuration) instance repository. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.data_dir)cftuconf.dat (not UNIX, not NT) &#124; $(cft.runtime.data_dir)cftparm (UNIX, NT)&quot; | not MVS |   |
| cft.uconf.instance_version | Version of the UCONF instance file. | string |   | ' ' | ALL | READ_ONLY |
| cft.uconf.runtime_fname | Path to uconf (unified configuration) runtime instance repository. | fname | &quot;min length:0, max length:512&quot; | &quot;DD:UCONFRUN (MVS) &#124; $(cft.runtime.run_dir)cftuconf-run.dat (not UNIX, not NT, not MVS) &#124; $(cft.runtime.data_dir)cftparm (UNIX, NT)&quot; | ALL |   |
| cft.unix.cftsu.afunix | Sets the AF_UNIX file for CFTSU. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)SCFTSU | UNIX | EXPERT |
| cft.unix.cftsu.fname | Specify the absolute pathname to the cftsu to execute. | fname | &quot;min length:0, max length:512&quot; |   | UNIX | EXPERT |
| cft.unix.cftsu.isservice | Use CFTSU as a service. | bool No Yes |   | No | UNIX |   |
| cft.unix.group_fname | Group filename for xfbadmutil. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.conf_dir)group | UNIX | EXPERT |
| cft.unix.group_temp | Temporary group filename for xfbadmutil. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.conf_dir)groupXXXXXX | UNIX | EXPERT |
| cft.unix.parse_file_name_suffix | Extract the suffix from filenames (Windows behavior). | bool Yes No |   | No | UNIX |   |
| cft.unix.passwd_fname | Password filename for xfbadmutil | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.conf_dir)passwd | UNIX | EXPERT |
| cft.unix.passwd_temp | Temporary password filename for xfbadmutil. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.conf_dir)passwdXXXXXX | UNIX | EXPERT |
| cft.unix.readline.enable | Enables readline history for CFTUTIL and PKIUTIL. | bool Yes No |   | $(cft.readline.enable) | UNIX |   |
| cft.unix.readline.history_fname | Readline history file. | fname | &quot;min length:0, max length:512&quot; | $(cft.readline.history_fname) | UNIX |   |
| cft.unix.readline.history_size | Readline history size. | int |   | $(cft.readline.history_size) | UNIX |   |
| cft.unix.shared_memory_size | Size (ko) of shared memory used by Transfer CFT. Advanced users only. | int |   | 32000 | UNIX | EXPERT |
| cft.unix.start_timeout | &quot;Sets the Transfer CFT time limit to start up. If the Transfer CFT status does not change within the delay, it is assumed that the 'cft start' command failed.&quot; | int |   | 30 | UNIX |   |
| cft.unix.stcp.afunix | Replaces an environment variable. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.data_dir)S_TCP | UNIX |   |
| cft.unix.stop_timeout | &quot;Sets the Transfer CFT time limit to stop. If the Transfer CFT status does not change within the delay, it is assumed that the stop failed.&quot; | int |   | 30 | UNIX |   |
| cft.unix.sx25.afunix | Replaces an environment variable. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.data_dir)S_X25 | UNIX |   |
| cft.unix.throw_error_on_envvar_not_found | &quot;Throw an error if a used environment variable does not exist. However, if you use '$' in filenames, this option must be set to NO for the file to be created.&quot; | bool Yes No |   | Yes | UNIX |   |
| cft.working_dir | Transfer CFT and GUI working directory. | dir | &quot;min length:0, max length:512&quot; | $(cft.runtime_dir) | ALL |   |
| cg.ca_cert_id | Identifier of the CA certificate (stored in the internal PKI base) used to authenticate the Central Governance server. | string list |   |   | ALL |   |
| cg.cert.subject_dn | Subject Distinguished Name of the Central Governance certificate. It's used to verify the Central Governance identity when Central Governance connects to Transfer CFT. | string |   | '' | ALL |   |
| cg.certificate.business.csr_dn | Distinguished Name to use to build the Certificate Sign Request for business flow. Tokens must be separated by a comma. | string |   | &quot;'O=Axway,OU=MFT,CN=$(cft.full_hostname)'&quot; | ALL | IRECONFIG |
| cg.certificate.business.csr_id | Internal identifier given by the Central Governance server when sending a Certificate Signing Request for business exchanges. | int |   | -1 | ALL | READ_ONLY |
| cg.certificate.business.key_len | &quot;Business certificate key length. When this parameter is changed, the \&quot; | int | enum: 2048 4096 | 2048 | ALL | EXPERT |
| cg.certificate.business.renewal_datetime | &quot;Datetime (GMT/UTC) used to force the renewal of the business certificate. When datetime is reached, start the renewal procedure. The datetime format is YYYYMMDDHHMMSS.&quot; | datetime |   |   | ALL | RECONFIG |
| cg.certificate.governance.csr_dn | Distinguished Name to use to build the Certificate Sign Request for governance exchanges. Tokens must be separated by a comma. | string |   | &quot;'O=Axway,OU=MFT,CN=Transfer CFT $(cft.instance_id)'&quot; | ALL | IRECONFIG |
| cg.certificate.governance.csr_id | Internal identifier given by the Central Governance server when sending a Certificate Signing Request for governance exchanges. | int |   | -1 | ALL | READ_ONLY |
| cg.certificate.governance.key_len | &quot;Governance certificate key length. When this parameter is changed, the \&quot; | int | enum: 2048 4096 | 2048 | ALL | EXPERT |
| cg.certificate.governance.renewal_datetime | &quot;Datetime (GMT/UTC) used to force the renewal of the governance certificate. When datetime is reached, start the renewal procedure. The datetime format is YYYYMMDDHHMMSS.&quot; | datetime |   |   | ALL | RECONFIG |
| cg.configuration_policy | Central Governance configuration policy to apply on the Transfer CFT instance. | string | &quot;min length:0, max length:100&quot; | ' ' | ALL |   |
| cg.enable | &quot;Enables exchanges with the Central Governance server (registration, heartbeat).&quot; | bool No Yes |   | No | ALL |   |
| cg.host | Central Governance server host address (IP address or FQDN). | string |   | 'cg-server-hostname' | ALL | RECONFIG |
| cg.key_label | Label used for License Key Management. | string |   | ' ' | ALL | EXPERIMENTAL |
| cg.metadata | Properties sent to Central Governance. | string list |   | agent | ALL |   |
| cg.metadata.&lt;logical_name&gt;.value | Value of the metadata. | string |   |   | ALL |   |
| cg.mutual_auth_port | Transfer CFT's port for communication with CG | int |   | 12554 | ALL | RECONFIG |
| cg.periodicity | &quot;Interval in seconds between two notifications (registration, heartbeat, certificate renewal) sent to Central Governance. When the &quot;&quot;Central Governance is waiting for approval&quot;&quot; option is enabled, the first notification interval is \&quot; | int |   | 600 | ALL | RECONFIG |
| cg.port | Transfer CFT's port for registering with CG | int |   | 12553 | ALL | RECONFIG |
| cg.proxy.in.host | Proxy host used by Central Governance to connect to Transfer CFT | string |   | '' | ALL | RECONFIG |
| cg.proxy.in.login | Proxy login used by Central Governance to connect to Transfer CFT | string |   | '' | ALL | RECONFIG |
| cg.proxy.in.password | Proxy password used by Central Governance to connect to Transfer CFT | passwd |   |   | ALL | RECONFIG |
| cg.proxy.in.port | Proxy port used by Central Governance to connect to Transfer CFT | int |   |   | ALL | RECONFIG |
| cg.proxy.out.host | Proxy host used by Transfer CFT to connect to Central Governance | string |   | '' | ALL | RECONFIG |
| cg.proxy.out.login | Proxy login used by Transfer CFT to connect to Central Governance | string |   | '' | ALL | RECONFIG |
| cg.proxy.out.password | Proxy password used by Transfer CFT to connect to Central Governance | passwd |   |   | ALL | RECONFIG |
| cg.proxy.out.port | Proxy port used by Transfer CFT to connect to Central Governance | int |   |   | ALL | RECONFIG |
| cg.registration_id | &quot;Registration Identifier provided by Central Governance server. If set to -1, Transfer CFT tries to register to Central Governance server.&quot; | int |   | -1 | ALL | EXPERT |
| cg.renewal_period | Number of day before expiration the certificate renewal procedure executes. | int |   | 60 | ALL | RECONFIG |
| cg.shared_secret | The shared secret needed to register to the Central Governance server. | passwd |   |   | ALL | RECONFIG |
| cg.timeout | TCP connection timeout (in seconds) | int |   | 5 | ALL | RECONFIG |
| copilot.batches | List of available batches (logical names separated by a white space). | string list |   | &quot;wlog cftext (NT, UNIX, OS400, MVS)&quot; | ALL |   |
| copilot.batches.&lt;logical_name&gt;.comment | Free comment. | string |   |   | ALL |   |
| copilot.batches.&lt;logical_name&gt;.file | Filename of the script. | fname | &quot;min length:0, max length:512&quot; |   | ALL |   |
| copilot.batches.&lt;logical_name&gt;.params | List of arguments to pass when calling the script. | string |   |   | ALL |   |
| copilot.batches.xmldefinition | The URL for the XML file used to customize the GUI. | string |   | 'guiextensions/plugin.xml' | ALL |   |
| copilot.catalog.paging | Selects paging mode. | bool Yes No |   | Yes | ALL |   |
| copilot.cft.com | &quot;Communication media description for JPI (medium Type = medium Name ex &quot;&quot;F=$CFTCOM&quot;&quot;)&quot; | string |   | ' ' | ALL |   |
| copilot.cft.maxcatrows | Limits the number of displayed catalog rows to this number (applies only if copilot.catalog.paging is set to No). | int |   | 5000 | ALL |   |
| copilot.cft.maxlogrows | Limits the number of displayed log rows to this number. | int |   | 10000 (MVS) &#124; 5000 (not MVS) | ALL |   |
| copilot.cft.mediacom | Selects the type of media to use. | string | enum: FILE TCPIP MBX | ' ' | ALL |   |
| copilot.cft.medianame | Selects the name of the media to use. | string |   | ' ' | ALL |   |
| copilot.cft.nbwaitcftcata | Wait number for catalog file scanning. | int |   | 6 | ALL |   |
| copilot.cft.timerwaitcftcata | Interval of time in seconds between catalog file scanning. | int |   | 5 | ALL |   |
| copilot.coms_proxy.recovery_delay | Delay in seconds between two attempts to recover from a connection problem. | int |   | $(copilot.node_manager.watchperiod) | ALL |   |
| copilot.coms_proxy.watchperiod | Interval in seconds between two checks of Transfer CFT coms configuration and status. | int |   | 60 | ALL |   |
| copilot.connection_dispatcher.control.port | Connection dispatcher TCP control port. | int |   | 0 | ALL |   |
| copilot.connection_dispatcher.enable | Enables client registering connection dispatching. | bool Yes No |   | No | ALL |   |
| copilot.connection_dispatcher.listen_ipv6_first | Ensures that listening is performed first on IPv6 resolutions. | bool Yes No |   | Yes | ALL | EXPERT |
| copilot.connection_dispatcher.listen_ipv6_v6only | Ensures that listening on IPv6 resolutions only apply to IPv6 calls. | bool Yes No |   | No | ALL | EXPERT |
| copilot.connection_dispatcher.local_port | Local TCP port for connection dispatching. | int |   |   | &quot;NT, UNIX, VMS&quot; | EXPERT |
| copilot.connection_dispatcher.retry_delay | The amount of time in seconds between retries for the client to register to the connection dispatcher. | int |   | 5 | ALL |   |
| copilot.connection_dispatcher.retry_timeout | The amount of time in seconds of the timeout when client is registering to the connection dispatcher. | int |   | 300 | ALL |   |
| copilot.connection_dispatcher.unix.af_unix_fname | Unix socket pathname for connection dispatching. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime_dir)/run/S_$(cft.short_hostname)DISPATCH | &quot;UNIX, OS400&quot; | EXPERT |
| copilot.general.enable | Enables use of the Copilot server. | bool Yes No |   | Yes | ALL |   |
| copilot.general.localport | Copilot notifications local listening port. | string |   | ' ' | ALL |   |
| copilot.general.login_failures.fname | Path to local file used to store login failure data. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)cftlogin.dat (not OS400) &#124; *LIBL/CFTLOGIN (OS400) | &quot;not MVS, not VMS&quot; |   |
| copilot.general.login_failures.max | Maximum number of login failure before locking the user for 30 seconds. Value 0 disables the feature. | int |   | 3 | &quot;not MVS, not VMS&quot; |   |
| copilot.general.persistencedir | Persistent data directory. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime_dir)/persist (not MVS, not VMS) &#124; $(cft.runtime_dir)persist] (VMS) &#124; $(copilot.HTTP.HttpRootDir)/persist (MVS)&quot; | ALL |   |
| copilot.general.serverhost | Copilot server listening address. | string |   | '0.0.0.0' (not VMS) &#124; '$(ucx$inet_host).$(ucx$inet_domain)' (VMS) | ALL |   |
| copilot.general.serverport | Copilot server listening port. | int |   | 1766 | ALL |   |
| copilot.general.ssl_serverport | Copilot (GUI) server listening port. | int |   | 1767 | ALL |   |
| copilot.http.aliases | &quot;List of enabled alias IDs, used to customize access to system directories.&quot; | string list |   | wsdl restapidocs restapiui cftui | ALL |   |
| copilot.http.cache | Configure HTTP cache header. | string list |   | javascript | ALL | EXPERT |
| copilot.http.cache.&lt;logical_name&gt;.expires | Cache validity. | string |   |   | ALL |   |
| copilot.http.cache.&lt;logical_name&gt;.filter | File pattern filter. | string |   |   | ALL |   |
| copilot.http.httprootdir | HTTP server root directory. | dir | &quot;min length:0, max length:512&quot; | $(cft.runtime_dir)/wwwroot (not VMS) &#124; $(cft.runtime_dir)wwwroot] (VMS) | ALL |   |
| copilot.http.onlyssl | If set to Yes disables use of the HTTP server when a SSL context is defined (HTTPS only). | bool Yes No |   | No | ALL |   |
| copilot.mftnavigator.enable | Enables remote Transfer CFT Navigators to connect to the Transfer CFT GUI. | bool Yes No |   | Yes | ALL |   |
| copilot.misc.backuplanguages | Ordered list of languages to use if information is missing in the used language. | string |   | en fr | ALL | EXPERT |
| copilot.misc.cftstart | Exec filename to start Transfer CFT. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.install_dir)/distrib/copilot/copcftstart (not NT, not MVS, not VMS) &#124; $(cft.install_dir)/distrib/copilot/copcftstart.bat (NT) &#124; $(cft.runtime_dir)INSTALL(CFTMON) (MVS) &#124; COPCFTSTART (VMS) &#124; *LIBL/COPCFTSTR (OS400)&quot; | ALL |   |
| copilot.misc.cftstart.enable | Enables the Start and Stop of CFT with Copilot. | bool Yes No |   | No | &quot;MVS, OS400&quot; |   |
| copilot.misc.cftstop | Script used by Copilot to stop Transfer CFT. | fname | &quot;min length:0, max length:512&quot; |   | UNIX |   |
| copilot.misc.childprocesstimeout | &quot;Inactivity timeout of child processes, in seconds.&quot; | int |   | &quot;20 (not MVS, not VMS, not OS400) &#124; 3600 (MVS) &#124; 60 (VMS) &#124; 86400 (OS400)&quot; | ALL |   |
| copilot.misc.client_keep_alive_delay | The amount of time in seconds between two sending of keep alive. The value 0 indicates no keep alive. | int |   | 60 | ALL |   |
| copilot.misc.clienttimeout | &quot;Client connection timeout, in minutes (0 means no timeout ; for CFT UI tokens, 0 means one hour timeout).&quot; | int |   | 30 | ALL |   |
| copilot.misc.copstart | Script used by Copilot to start Copilot. | fname | &quot;min length:0, max length:512&quot; |   | UNIX |   |
| copilot.misc.copstop | Script used by Copilot to stop Copilot. | fname | &quot;min length:0, max length:512&quot; |   | UNIX |   |
| copilot.misc.createprocessasuser | Enables using system users in Copilot. | bool Yes No |   | Yes (WIN-IA64) &#124; Yes (WIN-X86) &#124; No (UNIX) &#124; Yes (OS400) &#124; Yes (MVS) | ALL |   |
| copilot.misc.help_url | The URL for the Transfer CFT User guide. | string |   | 'https://docs.axway.com/bundle/TransferCFT_39_UsersGuide_allOS_en_HTML5/page/Content' | ALL |   |
| copilot.misc.languages | List of the additional languages. | string |   | '' | ALL | EXPERT |
| copilot.misc.local_encoding | &quot;Local encoding to use, which must be relatively compatible with ASCII.&quot; | string |   | AUTO (not VMS) &#124; iso8859-1 (VMS) | ALL |   |
| copilot.misc.maxnbprocess | Maximum number of processes in processes pool that may be ready to process a client connection. | int |   | 20 | ALL |   |
| copilot.misc.minnbprocessready | &quot;Minimum number of processes, in the processes pool, that must be ready to process a client connection.&quot; | int |   | 1 (not MVS) &#124; 2 (MVS) | ALL |   |
| copilot.misc.nbprocesstostart | &quot;Number of processes to add in processes pool, when there are not enough processes in the pool.&quot; | int |   | 1 (not MVS) &#124; 3 (MVS) | ALL |   |
| copilot.misc.server_inactivity_timeout | Inactivity timeout in seconds for copilot server process servicing a client (0 means no timeout). | int |   | 7200 | ALL |   |
| copilot.misc.tcptimeout | Timeout of TCP select in seconds. | int |   | &quot;10 (not MVS, not VMS) &#124; 60 (VMS) &#124; 300 (MVS)&quot; | ALL |   |
| copilot.misc.xts_encoding | XTS string encoding. | string |   | UTF-8 (not OS400) &#124; 01208 (OS400) | ALL | EXPERT |
| copilot.node_manager.node_repair_period | Delay in seconds used by CFT to detect and try to repair node starting issues. The value 0 means that the repair feature is disabled. It's recommended to set a value &gt;= 60 and &gt;= 4*copilot.node_manager.watchperiod. | int |   | 0 | ALL | EXPERT |
| copilot.node_manager.watchperiod | Interval in seconds between two checks of Transfer CFT nodes status. | int |   | 10 | ALL |   |
| copilot.nt.coptray_trcfilename | Coptray trace. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)coptray.trc | NT |   |
| copilot.nt.service_mode | Copilot is installed in service mode | bool Yes No |   | No | NT |   |
| copilot.nt.service_name | Copilot (GUI) service name in Windows. | string |   | CopilotService | NT |   |
| copilot.output.fname | Copilot (Transfer CFT UI server) output filename. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)copsmng.out | ALL | EXPERT |
| copilot.pid_fname | File containing the Copilot (copsmng) Process ID. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)copsmng.pid | ALL | EXPERT |
| copilot.restapi.api_token_validity | &quot;Specifies the API Token validity period, in seconds. 0 disables expiration.&quot; | int | min:0 | 0 | ALL |   |
| copilot.restapi.authentication_method | REST API Server Authentication method. | string |   | 'system' (not UNIX) &#124; 'xfbadm' (UNIX) | ALL |   |
| copilot.restapi.catalog.retry_delay | The amount of time in seconds between retries for the catalog surveys. | int | &quot;min:-1, max:10&quot; | 5 | ALL |   |
| copilot.restapi.catalog.retry_timeout | The amount of time in seconds of the timeout to look an update in the catalog. | int | &quot;min:-1, max:120&quot; | 30 | ALL |   |
| copilot.restapi.coms_id | The TCP CFTCOM object identifier used by the REST API server to communicate to Transfer CFT server. | string | &quot;min length:0, max length:32&quot; | 'coms' | ALL |   |
| copilot.restapi.enable | Enable or not rest API | bool Yes No |   | No | ALL |   |
| copilot.restapi.maxclient | Number of client connections handled per REST worker. | int | &quot;min:1, max:1024&quot; | 256 | ALL |   |
| copilot.restapi.nb_workers | Number of activated workers that process REST API requests. | int | &quot;min:1, max:10&quot; | 4 | ALL |   |
| copilot.restapi.serverport | The server address | int |   | 1768 | ALL |   |
| copilot.restapi.unix_socket_fname | Path to socket file (internal). | fname | &quot;min length:0, max length:256&quot; | $(cft.runtime.run_dir)S_COPRESTS | UNIX | EXPERT |
| copilot.rootdirs | List of authorized root directories used to limit remote file access. | string list |   | runtime | ALL |   |
| copilot.rootdirs.&lt;logical_name&gt;.fname | Path to accessible directory. | dir | &quot;min length:0, max length:512&quot; | ##INVALID## | ALL |   |
| copilot.run.control_port | Copilot main agent (copsmng) control port. | int |   |   | ALL | RUNTIME |
| copilot.run.copcod_local_port | Connection dispatcher local port in multi-node. | int |   |   | ALL | RUNTIME |
| copilot.run.copui_client_socket | Windows socket passing. | string |   | ' ' | ALL | RUNTIME |
| copilot.run.copui_client_ssl_socket | Windows ssl socket passing. | string |   | ' ' | ALL | RUNTIME |
| copilot.run.copui_notification_port | Notification port for copsmng (Copilot). | int |   |   | ALL | RUNTIME |
| copilot.run.copui_pid | copui Process ID. | string |   | '' | ALL | RUNTIME |
| copilot.run.pid | Copilot (copsmng) Process ID copsmng. | string |   | '' | ALL | RUNTIME |
| copilot.run.state | Transfer UI server status (Copilot). | string | enum: INITIALIZING STARTING RUNNING STOPPING STOPPED ERROR | '' | ALL | RUNTIME |
| copilot.run.state_timestamp | Last time copilot status changed. | string |   | '' | ALL | RUNTIME |
| copilot.ssl.sslcertfile | Filename of a PKCS12 file or X509 certificate file used to authenticate the Copilot server to clients. | fname | &quot;min length:0, max length:512&quot; |   | ALL |   |
| copilot.ssl.sslcertpassword | SSLCertFile (SSL certificate file) password. | passwd |   |   | ALL |   |
| copilot.ssl.sslciphersuites | Cipher suites to be used by Copilot server for https. | int_list |   | $(ssl.ciphersuites) | ALL |   |
| copilot.ssl.sslkeyfile | Separate key file needed if SSLCertFile is an X509 certificate. | fname | &quot;min length:0, max length:512&quot; |   | ALL |   |
| copilot.ssl.sslkeypassword | SSLKeyFile password. | passwd |   |   | ALL |   |
| copilot.ssl.version_min | Minimum SSL version allowed by Copilot. | string | enum: ssl_3.0 tls_1.0 tls_1.1 tls_1.2 | '$(ssl.version_min)' | ALL |   |
| copilot.startup.catalog | Open catalog on startup. | bool Yes No |   | Yes | ALL |   |
| copilot.startup.catalog.filter | Default startup catalog filter. | string |   | errors | ALL |   |
| copilot.startup.log | Open log on startup. | bool Yes No |   | Yes | ALL |   |
| copilot.startup.log.filter | Default startup log filter. | string |   | '' | ALL |   |
| copilot.trace | Specific trace level. | string list |   | TCP SSL HTTP XTS WS SYS FMK NOTF TRC CONF CFTF CFTS CFTE IUI | ALL |   |
| copilot.trace.&lt;logical_name&gt;.level | Trace level by category | string | enum: ERR WRN INF DBG | $(copilot.trace.trclevel) | ALL |   |
| copilot.trace.exec | Switchlog script. Sample script $(cft.runtime_dir)/exec/switchlog.cmd[bat]. | fname | &quot;min length:0, max length:512&quot; |   | ALL |   |
| copilot.trace.exec_args | Arguments for switchlog script. | string |   | '' | ALL |   |
| copilot.trace.exec_bufsize | Internal buffer. | int |   | 5 | ALL |   |
| copilot.trace.exec_time | Hour of day to execute switchlog. If not empty (default) use the YYYYmmddHHMMSS format. | string |   | '' | ALL | RUNTIME |
| copilot.trace.exec_timeout | Timeout in seconds for executing a switchlog. | int |   | 10 | ALL |   |
| copilot.trace.trcafilename | Alternate trace file of copui process. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)copui-a.trc | not MVS |   |
| copilot.trace.trcfilename | Primary trace file of copui process. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)copui.trc (not MVS) &#124; 'dd:COTRACE' (MVS) | ALL |   |
| copilot.trace.trclevel | &quot;General trace level: ERR(error), WRN(warning), INF(info), DBG(debug)&quot; | string | enum: ERR WRN INF DBG | 'ERR' | ALL |   |
| copilot.trace.trcmaxlen | Maximum size (Mb) of the primary trace file (dynamic parameter). | int |   | 10 | ALL |   |
| copilot.trace.trcshowline | Adds source file line number when using a trace. | bool Yes No |   | No | ALL | EXPERT |
| copilot.trace.trctype | Categories of events to trace (separator = SPACE). | string |   | 'TCP SSL HTTP XTS WS SYS FMK NOTF TRC CONF CFTF CFTS CFTE IUI' | ALL |   |
| copilot.unix.cftsu.afunix | Sets the AF_UNIX file for cftsu. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)Scftsu | UNIX | EXPERT |
| copilot.unix.cftsu.fname | Specify the absolute pathname to the cftsu to execute. | fname | &quot;min length:0, max length:512&quot; |   | UNIX | EXPERT |
| copilot.unix.cftsu.pid_fname | File containing the cftsu Process ID. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)cftsu.pid | UNIX | EXPERT |
| copilot.unix.unix_socket_fname | Path to socket file (internal). | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)S_COPSMNGFW | &quot;UNIX, OS400&quot; | EXPERT |
| copilot.update_manager.working_dir | Working directory of the Copilot Update Manager. | fname |   |   | UNIX | EXPERT |
| copilot.webservices.buffersoap | &quot;If set to Yes, the correct order of SOAP reply is buffered.&quot; | bool Yes No |   | No | ALL |   |
| copilot.webservices.enable | Enables the use of Web services. | bool Yes No |   | Yes | ALL |   |
| copilot.webservices.ordersoap | &quot;If set to Yes, the SOAP reply has the order defined in the copilotcft.wsdl.&quot; | bool Yes No |   | No | ALL |   |
| copilot.webservices.upload_directory | Directory where files are uploaded. | dir | &quot;min length:0, max length:512&quot; | $(cft.runtime_dir)/upload (not MVS) &#124; $(cft.runtime_dir)UPLOAD (MVS) &#124; $(cft.install_dir)UPLOAD] (VMS) | ALL |   |
| copilot.webservices.wsicomplience | &quot;If set to Yes, client requests are checked against WS-I recommendations.&quot; | bool Yes No |   | No | ALL |   |
| crypto.key_fname | Filename containing the private key to use to encipher data. | fname |   |   | ALL |   |
| crypto.salt_fname | Filename containing the salt used to create the private key. | fname |   |   | ALL |   |
| custom.install | Custom variables used in samples. | string list |   |   | not MVS |   |
| folder_monitoring.enable | Enable the folder monitoring functionality. | bool No Yes |   | No | ALL | EXPERT |
| folder_monitoring.folders | List of directory to scan attributes sets. | string list |   |   | ALL | EXPERT |
| folder_monitoring.folders.&lt;logical_name&gt;.control | Meta data used to control user changes. | string |   |   | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.enable | Enables the scan of the folder. | bool No Yes |   | Yes | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.enable_subdir | Enables sub-directory scan. | bool No Yes |   | Yes | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.file_count | Number of file to take into account while scanning. | int |   | -1 | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.file_exclude_filter | Exclude file name matching this pattern. | string |   |   | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.file_idle_delay | Delay in seconds to decide that a file is ready to be submitted. | int |   | 5 | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.file_include_filter | Include file name matching this pattern. | string |   |   | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.file_size_max | Maximum file size in bytes - ignored if larger size. | int |   | -1 | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.file_size_min | Minimum file size in bytes - ignored if shorter size. | int |   | -1 | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.filter_type | Pattern filtering algorithm to use. | string | enum: STRJCMP WILDMAT EREGEX |   | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.idf | CFT IDF either fixed string or sub-directory name level (0) or (1). | string |   |   | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.interval | Interval between two scans in seconds. | int |   | 60 | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.method | Monitoring method (file or move). MOVE method recommended. | string | enum: FILE MOVE | MOVE | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.part | CFT PART either fixed string or sub-directory name level (0) or (1). | string |   |   | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.renaming_method | &quot;If TIMESTAMP, add a timestamp (GMT/UTC) to the moved file name.&quot; | string | enum: TIMESTAMP NONE | TIMESTAMP | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.renaming_separators | Character before [and after] the timestamp in file name. | string |   |   | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.resubmit_changed_file | Resubmit a file changed after previous submission (FILE method only). | bool No Yes |   | Yes | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.scan_dir | Top level directory to scan. | string |   |   | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.use_file_system_events | Enables use of the file system events monitoring service. | bool No Yes |   | No | ALL |   |
| folder_monitoring.folders.&lt;logical_name&gt;.work_dir | Directory specification where to move files from scan_dir. | string |   |   | ALL |   |
| pesit.parameters.enable_encoding | &quot;Defines if the PeSIT parameters should be encoded in UTF-8. This affects PI37, PI91 and PI99.&quot; | bool Yes No |   | No | &quot;UNIX, NT&quot; | EXPERT |
| pki.cft.port | Transfer CFT PKI listening server port number. Value 0 means a dynamic port will choose by the Operating System at runtime. Other values will be handled as a range from port to (port + number of nodes - 1) in multinode. | int |   | 0 | ALL |   |
| pki.exit.libpath | Location of CFTPKIE exit dynamic library. | fname | &quot;min length:0, max length:512&quot; |  &#124; CFTPKIE (VMS) | ALL |   |
| pki.passport.hostname | PassPort PKI server IP address. | string |   | 'localhost' | ALL |   |
| pki.passport.login | PassPort PKI login. Leave empty for an anonymous connection. | string |   | '' | ALL |   |
| pki.passport.msgpath | PassPort msgpath (full pathname of the message-log file). | fname | &quot;min length:0, max length:512&quot; | $(cft.install.psenglish_dir) (not MVS) &#124; 'dd:PKIMSG' (MVS) | ALL | EXPERT |
| pki.passport.password | PassPort PKI password. | passwd |   |   | ALL |   |
| pki.passport.port | PassPort PKI server port number. | int |   | 7000 | ALL |   |
| pki.passport.trace | XPP PKI trace (0-&gt;5). | string |   | '0' | ALL |   |
| pki.run.cft | Number of defined Secure Relay Master Agents. | int |   | 1 | ALL |   |
| pki.run.cft.&lt;itemNumber&gt;.port | Transfer CFT PKI listening server port number reserved at runtime for a specific node. | int | &quot;min:1, max:65535&quot; | 0 | ALL |   |
| pki.run.cft.port | Transfer CFT PKI listening server port number reserved at runtime. | int |   | 0 | ALL | RUNTIME |
| pki.type | PKI type used. | string |   | 'cft' | ALL |   |
| samples | Parameters used for the sample configuration file (cft-tcp.conf). | string list |   |   | ALL |   |
| secure_relay.enable | Enables Secure Relay. | bool Yes No |   | No | ALL |   |
| secure_relay.ma.admin_outport_range | Secure Relay Master Agent admin outport range. | string |   | ' ' | ALL | EXPERT |
| secure_relay.ma.autostart | Allows to automatically start the embedded Secure Relay Master Agent. | bool Yes No |   | Yes | ALL |   |
| secure_relay.ma.ca_cert_fname | Secure Relay certificate authority. | fname | &quot;min length:0, max length:512&quot; |   | ALL |   |
| secure_relay.ma.cert_fname | Secure Relay Master Agent user certificate. | fname | &quot;min length:0, max length:512&quot; |   | ALL |   |
| secure_relay.ma.cert_password | Secure Relay Master Agent certificate password. | passwd |   | test | ALL |   |
| secure_relay.ma.cert_password_fname | Secure Relay Master Agent certificate password file. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)XsrPwd.dat (not MVS) &#124; $(cft.runtime.xsr_dir)XsrPwd.dat (MVS) | ALL |   |
| secure_relay.ma.comm_outport_range | Secure Relay Master Agent comm outport range. | string |   | ' ' | ALL | EXPERT |
| secure_relay.ma.comm_port | &quot;Secure Relay Master Agent listening communication port. In multinode, this will be handled as a range from port to (port + number of nodes - 1).&quot; | int | &quot;min:1, max:65535&quot; | 6801 | ALL |   |
| secure_relay.ma.conf_fname | Secure Relay Master Agent configuration file. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)XsrConf.xml (not MVS) &#124; $(cft.runtime.xsr_dir)XsrConf.xml (MVS) | ALL |   |
| secure_relay.ma.heartbeat_service.host | Heartbeat service listening server IP address or FQDN for Secure Relay Master Agent. | string | &quot;min length:0, max length:512&quot; | '127.0.0.1' | ALL |   |
| secure_relay.ma.heartbeat_service.periodicity | Interval in seconds between two heartbeats. | int | &quot;min:1, max:86400&quot; | 30 | ALL |   |
| secure_relay.ma.heartbeat_service.port | Heartbeat service listening server port number for Secure Relay Master Agent. Value 0 means a dynamic port will choose by the Operating System at runtime. Other value will be handled as a range from port to \ | int | &quot;min:1, max:65535&quot; | 0 | ALL |   |
| secure_relay.ma.host | Secure Relay Master Agent listening IP address or FQDN. | string |   | '127.0.0.1' | ALL |   |
| secure_relay.ma.jar_fname | Secure Relay Master Agent jar file. | fname | &quot;min length:0, max length:512&quot; | $(cft.install.xsr_dir)xsrMaster.jar | ALL |   |
| secure_relay.ma.log_fname | Secure Relay Master Agent log file. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.log_dir)xsrMaster.log (not MVS) &#124; $(cft.runtime.xsr_dir)XsrMaster.log (MVS) | ALL |   |
| secure_relay.ma.log_level | &quot;Secure Relay Master Agent log level : 0, 1, 2 or 3.&quot; | int | min:0; max:3 | 0 | ALL |   |
| secure_relay.ma.pid_fname | File containing the Secure Relay Master Agent Process ID. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)xsrMaster.pid (not MVS) &#124; (MVS) | ALL |   |
| secure_relay.ma.start_options | Secure Relay Master Agent start options. | string |   | &quot;'-Xmx1024m' (not NT, not MVS) &#124; '-Xmx512m -Xrs' (NT) &#124; '-Xms512m' (MVS)&quot; | ALL | EXPERT |
| secure_relay.ma.start_proc | Secure Relay Master Agent start procedure. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime_dir)INSTALL(XSRRUN) (MVS) &#124; *LIBL/STRXSR (OS400) | &quot;MVS, OS400&quot; |   |
| secure_relay.ma.start_timeout | Amount of time in seconds of the timeout when starting Secure Relay. | int |   | &quot;15 (not MVS, not OS400) &#124; 120 (MVS, OS400)&quot; | ALL |   |
| secure_relay.ra | Number of defined Secure Relay Router Agents. | int |   | 1 | ALL |   |
| secure_relay.ra.&lt;itemNumber&gt;.admin_port | Router Agent administration port. | int |   | 6810 | ALL |   |
| secure_relay.ra.&lt;itemNumber&gt;.comm_port | &quot;Router Agent communication port. In multinode, this will be handled as a range from port to (port + number of nodes - 1).&quot; | int |   | 6811 | ALL |   |
| secure_relay.ra.&lt;itemNumber&gt;.data_channel_ciphering | Enables data connections ciphering. | bool Yes No |   | No | ALL |   |
| secure_relay.ra.&lt;itemNumber&gt;.dmz | Logical name of the DMZ where the Router Agent is running (32 char max). | string |   | DMZ0 | ALL |   |
| secure_relay.ra.&lt;itemNumber&gt;.enable | Enables the Router agent. | bool Yes No |   | Yes | ALL |   |
| secure_relay.ra.&lt;itemNumber&gt;.host | Router Agent IP address or FQDN. | string |   |   | ALL |   |
| secure_relay.ra.&lt;itemNumber&gt;.nb_data_connections | Number of data connections between Master Agent and Router Agent. | int |   | 5 | ALL |   |
| secure_relay.ra.&lt;itemNumber&gt;.outcall_network_interface | Address to bind for outgoing calls. | string |   |   | ALL |   |
| secure_relay.run.ma | Number of defined Secure Relay Master Agents. | int |   | $(cft.multi_node.nodes) | ALL | RUNTIME |
| secure_relay.run.ma.&lt;itemNumber&gt;.heartbeat_service_port | Heartbeat service listening server port number reserved at runtime for a specific node. | int | &quot;min:1, max:65535&quot; | 0 | ALL |   |
| secure_relay.run.ma.heartbeat_service.port | Heartbeat service listening server port number reserved at runtime. | int | &quot;min:1, max:65535&quot; | 0 | ALL | RUNTIME |
| sentinel.heartbeat.enable | Enables sending Heartbeats to the Sentinel Server. | bool Yes No |   | No | ALL | RECONFIG |
| sentinel.heartbeat.periodicity | The delay in seconds between sending Heartbeats. | int |   | 300 | ALL | RECONFIG |
| sentinel.heartbeat.script | Script for executing Heartbeats. | fname | &quot;min length:0, max length:512&quot; | $(cft.install.extrasSENT_dir)MFTheartbeat.sh (UNIX) &#124; $(cft.install.extrasSENT_dir)MFTheartbeat.bat (NT) &#124; $(cft.install.extrasSENT_dir)MFTheartbeat.com (VMS) &#124; *LIBL/HEARTBEAT (OS400) | ALL | RECONFIG |
| sentinel.trk_max_port | Outgoing port range upper limit. | int |   | 32000 | ALL |   |
| sentinel.trk_max_port_bkup | Outgoing port range upper limit for the backup Server. | int |   | 32000 | ALL |   |
| sentinel.trk_min_port | Outgoing port range lower limit. | int |   | 5000 | ALL |   |
| sentinel.trk_min_port_bkup | Outgoing port range lower limit for the backup server. | int |   | 5000 | ALL |   |
| sentinel.trkflushdurationmax | Maximum amount of time in seconds to flush the overflow file. | int |   | 3 | ALL |   |
| sentinel.trkgmtdiff | Difference between local time and GMT in minutes. | int |   |   | ALL |   |
| sentinel.trkident | Application name for audit. | string |   | '$(cft.instance_id)' | ALL |   |
| sentinel.trkipaddr | Sentinel Server IP address. | string |   | 'sentinel-server-hostname' | ALL |   |
| sentinel.trkipaddr_bkup | Sentinel backup server IP address. | string |   | '' | ALL |   |
| sentinel.trkipport | Sentinel Server port number. | string |   | '1305' | ALL |   |
| sentinel.trkipport_bkup | Sentinel backup port number. | int |   | 1305 | ALL |   |
| sentinel.trklenmsg | Maximum length of a message in TrkUtil (default 16000). | int |   | 16000 | ALL |   |
| sentinel.trklocaladdr | Universal Agent IP Address. | string |   | '$(cft.full_hostname)' | ALL |   |
| sentinel.trkmsgencoding | Charset encoding of messages sent to Sentinel Server. | string | enum: ISO8859-1 ISO8859-15 UTF-8 | &quot;'ISO8859-1' (not MVS, not OS400) &#124; 'none' (MVS, OS400)&quot; | ALL |   |
| sentinel.trkproductipaddr | Application IP Address. | string |   | '$(cft.full_hostname)' | ALL |   |
| sentinel.trkproductname | Application name. | string |   | 'CFT' | ALL |   |
| sentinel.trksharedfile | MANDATORY when the Event Router and applications are sharing the logger file. Set to NO if applications are sending messages directly to the Sentinel server without going through an ER. | bool Yes No |   | No | MVS |   |
| sentinel.trktconnretry | Time in minutes between retries when TRKTMODE is set to RETRY. | int |   | 3 | ALL |   |
| sentinel.trktimeout | Timeout in seconds. | int |   | 60 | ALL |   |
| sentinel.trktmode | The mode that TrkUtil uses to send messages from TrkUtil to the Sentinel Server. | string | enum: DIFFER IMMEDIAT RETRY D I R | 'RETRY' | ALL |   |
| sentinel.trktname | The path to the overflow file. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.data_dir)trkapi.buf (not MVS, not VMS) &#124; $(cft.runtime.data_dir)TRKTAM.SEQ (VMS)&quot; | ALL |   |
| sentinel.trktrace | Level of trace (0=disabled). | int |   | 0 | ALL |   |
| sentinel.trktrcfile | Trace filename. | fname | &quot;min length:0, max length:512&quot; | &quot;$(cft.runtime.run_dir)sentinel.trc (not MVS, not OS400) &#124; (MVS) &#124; *LIBL/TRKTRC (OS400)&quot; | ALL |   |
| sentinel.trktype | Sentinel server connection type. | string | enum: TCP SOPIX | 'TCP' | ALL |   |
| sentinel.xfb.audit | Enables configuration change logging. | bool Yes No |   | No | ALL |   |
| sentinel.xfb.buffer_name | Overflow filename for a configuration audit. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.run_dir)xfbtrk.buf (not MVS) | ALL |   |
| sentinel.xfb.buffer_size | Maximum number of messages in buffer. | int |   | 100000 | ALL |   |
| sentinel.xfb.ca_cert_id | Identifier of the CA certificate (stored in the internal PKI base) used to authenticate the Sentinel server. | string list |   | $(cg.ca_cert_id) | ALL |   |
| sentinel.xfb.cyclelink.metadata | The model for the metadata attribute that Transfer CFT adds when it generates a Sentinel cycle link message. | string |   | ' ' | ALL | RECONFIG |
| sentinel.xfb.enable | Enables the Sentinel connector. | bool Yes No |   | No | ALL |   |
| sentinel.xfb.in_memory_pending_messages_max | Maximum pending messages in memory prior to logging alerts. | int |   | 0 | ALL |   |
| sentinel.xfb.log | Log Filter: (I)nfo (E)rror (W)arning (F)atal. | string |   | 'IEWF' | ALL |   |
| sentinel.xfb.shut | Overflow file fill level beyond which Transfer CFT shuts down (0 = no shutdown). | int |   | 0 | ALL |   |
| sentinel.xfb.testlabel | Specific test label. | int |   |   | ALL |   |
| sentinel.xfb.transfer | Level of detail for Transfer Message content. | string | enum: ALL SUMMARY NO ERROR | 'ALL' | ALL |   |
| sentinel.xfb.transfer.send_relay_site_nidf | Enables nidf on relay site | bool Yes No |   | No | ALL |   |
| sentinel.xfb.transfer_progress_period | Frequency in seconds in which Transfer CFT notifies Sentinel that a transfer is running (0 = never). | int |   | 60 | ALL | RECONFIG |
| sentinel.xfb.use_ssl | Enables SSL cryptography when connecting to Sentinel Server or ER. | bool Yes No |   | No | ALL |   |
| ssl.certificates.ca_cert_bundle | Path to the CA cert bundle.\ | string |   | '' | ALL |   |
| ssl.ciphersuites | SSL cipher suites negotiated by Transfer CFT connectors. | int_list |   | &quot;49200, 49199, 156, 157, 60, 61, 47, 53&quot; | ALL |   |
| ssl.extension.enable_sni | Enable TLS Server Name Indication extension for Transfer CFT connectors. | bool Yes No |   | Yes | ALL | EXPERT |
| ssl.version_min | Minimum SSL version allowed by Transfer CFT connectors. | string | enum: ssl_3.0 tls_1.0 tls_1.1 tls_1.2 | 'tls_1.0' | ALL |   |
| tf.defaultlocalcharset | Corresponds to a character set listed in the character set conversion reference table. | string |   | 'ISO-8859-1' | ALL | RECONFIG |
| tf.enablepasswordcipher | &quot;Indicates that entities passphrases, either in the entities definition file (entities.xml) or in the operation description file.&quot; | bool Yes No |   | Yes | ALL |   |
| tf.entitieslocation | Indicates the path to the entities.xml file where Trusted File is configured in standalone mode. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.conftf_dir)entities.xml (not MVS) &#124; $(cft.runtime.conftf_dir)UPARM(XENTITI) (MVS) | ALL |   |
| tf.entitieslocationtype | Trusted File configures a local or remote server (local or remote) | string |   | 'local' | ALL | RECONFIG |
| tf.messageslocation | Transfer CFT runtime directory. | dir | &quot;min length:0, max length:512&quot; | $(cft.install.tfenglish_dir) | ALL |   |
| tf.overwritemode | &quot;Defines how Axway TrustedFile behaves when it opens an existing plain file, acknowledgement or envelope in write mode.&quot; | string |   | 'enable' | ALL |   |
| tf.proofsenabled | Enables use of proofs. | bool Yes No |   | Yes | ALL |   |
| tf.proofslocation | References the absolute path to the directory that the product uses to generate proofs. | dir | &quot;min length:0, max length:512&quot; | $(cft.runtime.datatf_dir) | ALL |   |
| tf.proofsuidfile | &quot;ProofsUIDFile is used, when no proof is requested&quot; | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.datatf_dir)proofsUIDFile.cfg (not MVS) &#124; $(cft.runtime.datatf_dir)PROOFUID (MVS) | ALL |   |
| tf.trace | Specific trace level. | string list |   | mime pgp smime xasn xfm xpf xpki xpp xppwrap | ALL |   |
| tf.trace.&lt;logical_name&gt;.level | Trace level. | int |   | $(tf.trace.trclevel) | ALL |   |
| tf.trace.trclevel | Trace level: between 0 and 9 | int |   | 0 | ALL |   |
| tf.transcodingtablelocation | Absolute path to the character set conversion reference table. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.conftf_dir)transcoding.tbl (not MVS) &#124; $(cft.runtime.conftf_dir)UPARM(XTRANSC) (MVS) | ALL |   |
| trace.file.level | Use only under Axway support supervision. | int |   | 0 | ALL | EXPERIMENTAL |
| trace.net.level | Use only under Axway support supervision. | int |   | 0 | ALL | EXPERIMENTAL |
| trace.xtrace.default_fname | Use only under Axway support supervision. | fname | &quot;min length:0, max length:512&quot; | $(cft.runtime.traces_dir)cft.trc | ALL | EXPERIMENTAL |
| trace.xtrace.default_level | Use only under Axway support supervision. | fname | &quot;min length:0, max length:512&quot; | 0 | ALL | EXPERIMENTAL |

