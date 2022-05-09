{
    "title": "Changelog",
    "linkTitle": "Changelog",
    "weight": "40"
}****Transfer CFT 3.10****

- [CORE] Support 64-character identifiers for CFTSSH and PKIKEY.
- [CORE] Added a user audit log that tracks user authentication, administration, configuration, and denied transfer actions. [Details](../transport_security_start_here/user_auditing)
- [CORE] Support using a group of files to send to or receive from: Google Cloud Storage [Details](../app_integration_intro/google_cloud) or Amazon S3 [Details](../app_integration_intro/amazon_s3)
- [CORE] Support for Microsoft Azure Blob Storage and using a group of files to send to or receive from ABS. [Details](../app_integration_intro/ms_blob)
- [CORE] Added new UCONF `pki.expiration.check` parameters to enable warning concerning certificate expiration in the CFTLOG. [Details](../admin_intro/uconf/uconf_directory)
- [z/OS] Added a new process responsible to start and monitor scripts AND EXECS on z/OS platforms. Details
- [CORE] Added a new process that manages and monitors processing and exec scripts to improve transfer performance, most notably on z/OS systems.
- [UI] You can customize the columns when exporting a CSV file from the Transfer CFT UI. [Details](../c_intro_userinterfaces/web_copilot_ui/operations/managing_transfer_states)

****Transfer CFT 3.9****

- [CORE] A new parameter requires you to accept the Transfer CFT license agreement.
- [CONTAINER] Updated the Docker delivery to support OpenShift. [Details](../cft_intro_install/contanier_intro/install_container)
- [CORE] Added support for Google Cloud Storage when performing transfers using the SFTP protocol. [Details](../app_integration_intro/google_cloud)
- [Windows] Google Cloud Storage is available on Windows operating systems. [Details](../app_integration_intro/google_cloud)
- [IBM i] IBM i supports the SFTP protocol when transferring files. [Details](../protocols_start_here/sftp_intro)
- [CORE] Out-of-the-box support for removing files from the server after a RECV/CFTRECV pull operation when using SFTP. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/naction)
- [OpenVMS] Support for the user interface and REST API on OpenVMS systems.
- [OpenVMS] Delivered the OpenVMS Installation and Operation Guide. [Details](../cft_intro_install/about_this_document_vms)
- [UI] Added access management controls that limit a user's access to certain functions. [Details](../c_intro_userinterfaces/web_copilot_ui/home)
- [UI] You can generate a `cft-support` archive file from the **Support** page in the user interface (*UNIX and Windows*). [Details](../c_intro_userinterfaces/web_copilot_ui/operations/cft_support_ui)
- [UI] You can now export a list of transfers as a CSV output file from the user interface **Catalog** page. [Details](../c_intro_userinterfaces/web_copilot_ui/operations)
- [UI] Support for browsing the Transfer CFT server file system for a file via the user interface or REST API. [Details](../c_intro_userinterfaces/web_copilot_ui)
- [CORE] Add a root directory for browsing in the user interface. [Details](../c_intro_userinterfaces/web_copilot_ui)
- [IBM i] Updated the commands to update (UPDCFT), upgrade (UPGCFT), and patch (PTCCFT). Additionally, added a new command to roll back the version (RSTCFT). [Details](../cft_intro_install/about_this_document_ibmi/upgrade_prereqs_ibm_i)
- [CORE] Added statistical variables for LISTCOM and LISTCAT. [Details](../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/list_statistics)
- [CORE] Added the CFTUTIL LISTCOM CONTENT=STAT command to display statistical information about commands. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/content)
- [CORE] Added a date and time timestamp for each record in the communication media COM object. Details
- [CORE] Added `EventDateTime `and `EventTimestamp `attributes to XFBTransfer tracked object. [Details](../using_sentinel/intro_sentinel/xfb_to_attributes)
- [CORE] Activate integrity control in Unix, Hp NonStop, and Windows environments. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/mac)
- [CORE] Updated the help for relative time in CFTSEND/CFTRECV mintime/maxtime parameter definitions.
- [CORE] Updated the message categories as defined in opermsg. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/opermsg)
- [z/OS] Updated the access management security on z/OS systems. [Details](../cft_intro_install/about_this_document_zos/upgrade_prereqs_zos/security_migration_zos)
- [z/OS] Modified the maximum value of DIRNB to 999999. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/dirnb)

****Transfer CFT 3.8****

- [CORE] Added support for Google Cloud Storage. [Details](../app_integration_intro/google_cloud)
- [AM] Enhanced Internal Access Management to use roles and privileges defined in CFTPARM (as type CFTROLE and CFTPRIV objects) providing more flexibility than simply predefined roles. [Details](../internal_a_m_start_here/uconf_internal_am)
- [UI] The user interface additionally supports French, Portuguese, and Spanish.
- [UI] You can customize and resize the columns for the Transfer page in the user interface. [Details](../c_intro_userinterfaces/web_copilot_ui/operations/managing_transfer_states)
- [CORE] Added the CFTUIPREF object to customize Transfer page columns and filters. [Details](../c_intro_userinterfaces/command_summary#CFTUIPREF)
- [CORE] Select transfers with CFTUTIL using absolute and relative date and time criteria. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/datetimemin) (DATETIMEMIN) [Details](../c_intro_userinterfaces/command_summary/parameter_intro/datetimemax) (DATETIMEMAX)
- [UI] Removed support for the Transfer CFT Copilot UI (Java applet).
- [CORE] Enabled the cache feature by default. The default value for `cft.server.parm.cache_size` is now 5000 instead of zero. [Details](../cft_intro_install/mig_impact_considerations#parmcache)
- [CORE] Added the NIDF field in accounting files (for V24 structures). [Details](../admin_intro/admin_config_commands/cftaccnt_concepts)
- [IBM i] Included accounting C samples. [Details](../cft_intro_install/about_this_document_ibmi/post_install_intro_ibmi/api_and_exits_intro_ibmi)
- [CORE] Modified the minimum CFTLOG `length `parameter. [Details](../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftlog)
- [CORE] Replace proprietary database for configuration with SQLite database for Windows, UNIX, and HP NonStop.
- [CORE] Added support for EC2 instance profile credentials and AWS credentials file use with ACL, a proxy, or server-side encryption. [Details](../app_integration_intro/amazon_s3)
- [DOC] The User Guide is now available as a PDF. [Details](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_PDF/resource/Transfer_CFT_UsersGuide_allOS_en.pdf)
- [REST API] Added new query parameters to the GET /transfers REST API. See the Swagger documentation for [details](http://apidocs.axway.com/swagger-ui/index.html?productname=transfercft&productversion=3.8&filename=transfercft-swagger-api.json).

****Transfer CFT 3.7****

- [CORE] Added server-side encryption for AWS S3. [Details](../app_integration_intro/amazon_s3)
- [CORE] Added Access Control List (ACL) policy for uploading files to AWS S3. [Details](../app_integration_intro/amazon_s3)
- [CORE] Added HTTP/HTTPS proxy support for AWS S3. [Details](../app_integration_intro/amazon_s3)
- [CORE] Support for IPv6 while performing exchanges using SFTP.
- [SAMPLE] Google Cloud Storage Connector samples that use pre and post-processing scripts are now available on the [AMPLIFY Repository](https://repository.axway.com/product-extensions/views/google-cloud-storage-connector-sample-for-transfer-cft).
- [CORE] Added a beginning-of-transfer exit, EXITBOT, that allows you to customize certain fields in a request. [Details](../app_integration_intro/managing_exits/beginning_of_transfer_exit)
- [CORE] Added the cft.cftlog.switch_on_start parameter to enable the automatic LOG switch when Transfer CFT starts. When `cft.cftlog.switch_on_start=No`, the log file switches once per day instead of at each start. [Details](../admin_intro/admin_monitoring_intro/housekeeping_logs)
- [CORE] Support for environment variables in folder monitoring (CFTFOLDER) scandir and workdir parameters definitions using the %env:X% syntax. [Details](../c_intro_userinterfaces/command_summary/typographical_conventions)
- [CORE] Added the `cg.certificate.governance.csr_dn` parameter to enable users to customize the DN. Flow Manager [Details](../governance_services_intro/register_fm), Central Governance [Details](../governance_services_intro/register_cg)
- [CORE] Updated the CFTS03I log message, replacing the word “executed” with “submitted”. [Details](../troubleshoot_intro/messages_and_error_codes_start_here/cfts_messages)
- [UI] Support for the RECONFIG command and ACT/INACT commands.
- [UI] Creating a new transfer from the UI supports the same parameters as the CFTUTIL SEND/RECV commands.
- [REST API] Support for the RECONFIG command. Refer to the [Swagger documentation](http://apidocs.axway.com/swagger-ui/index.html?productname=transfercft&productversion=3.7&filename=transfercft-swagger-api.json)
- [REST API] Support for the ACT/INACT commands. Refer to the [Swagger documentation](http://apidocs.axway.com/swagger-ui/index.html?productname=transfercft&productversion=3.7&filename=transfercft-swagger-api.json)
- [SECURITY] Improved the LISTPKI display and functionality adding more information on certificate status and the ability to apply models to displayed information. [Details](../transport_security_start_here/certificates/pkiutil_cli_intro/using_the_listpki_command)
- [SECURITY] In access management, you can now use the EDIT and VIEW actions in the SERVICE:CFTSRV resource to control changes on a multi-node Transfer CFT instance (add_node, remove_node, disable_node, add_host, remove_host). [Details](https://docs.axway.com/bundle/TransferCFT_38_SecurityGuide_allOS_en_HTML5/page/Content/security_guide/iam.htm)
- [WINDOWS] Added support for PowerShell. You can now interact with cft command line (cft, CFTUTIL, PKIUTIL) using PowerShell by loading the profile.ps1 instead of profile.bat. [Details](../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows/getenv_windows)
- [WINDOWS] Added UNC network drive support for Transfer CFT runtime installations. [Details](../cft_intro_install/windows_install_start_here/before_you_start_win)
- [WINDOWS] Transfer CFT Windows requires the Visual C++ Redistributable Package for Visual Studio 2019 for proper functioning.
- [OS400] Added an automatic upgrade procedure. [Details](../cft_intro_install/about_this_document_ibmi/upgrade_prereqs_ibm_i/upgrade_ibmi)
- [SAMPLE] Support for Terraform. Please refer to the Terraform package available on [support.axway.com](https://support.axway.com/) for details.
- [CORE] Containerized application integration using Kubernetes. [Details](../cft_intro_install/contanier_intro/container_integration)
- [CORE] Updated the documentation for the account file in v24 format. Please note the changes in field length as described in the CFTACCNT list. [Details](../admin_intro/admin_config_commands/cftaccnt_concepts)

****Transfer CFT 3.6****

- [CORE] Enhanced the C language and COBOL end-of-transfer (EOT) exits. Details SP1
- [IBM i] Added SAML support on IBM i. Details SP1
- [HPNS] Added SAML signature support on HP NonStop IA-64. Details SP1
- [CORE] Added support for an outgoing HTTP proxy to connect to Central Governance or Flow Manager. Details (CG) or Details (FM) SP1
- [CORE] Updated the Docker delivery with a Helm chart sample for Kubernetes. SP1
- [CORE] SFTP support when using Transfer CFT with Secure Relay. [Details](../transport_security_start_here/sr_overview)
- [CORE] Added usage tracking with the AMPLIFY Edge Agent. [Details](../reporting)
- [CORE] Support Ceph Object Storage with S3. [Details](../app_integration_intro/amazon_s3#Use)
- [CORE] Added a folder monitoring option that allows you to access files on the behalf of a specific user (USERID). *Available on Unix and Windows* [Details](../app_integration_intro/intro_folder_monitor/configure_folder_monitoring)
- [CORE] You can automatically move a file to a specified directory following a successful file transfer either with folder monitoring [Details](), or without folder monitoring using FACTION [Details](../c_intro_userinterfaces/command_summary/parameter_intro/faction)
- [CORE] Workingdir is available for SEND and RECV commands. [Details](../app_integration_intro/amazon_s3)
- [CORE]) Added an alert mechanism for the Transfer CFT communication media file (CFTCOM) to indicate that the COM is nearing its threshold. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/tlvwarn)
- [CORE] Extended RCVALLER to include additional file-access type errors. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/rcvaller)
- [CORE] Enable/disable the PassPort AM persistent cache daemon (CFTSXPAM). [Details](../internal_a_m_start_here/about_passport_am/unconf_access_management)
- [CORE] Added the SORTBY parameter for the DISPLAY and LISTCAT commands. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/sortby)
- [CORE] Added filtering using the ROOTCID parameter for the LISTPKI and PKIEXT commands. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/rootcid)
- [CORE] Symbolic link support for an installation directory path. Windows [Details](../cft_intro_install/windows_install_start_here/before_you_start_win), and UNIX [Details](../cft_intro_install/unix_install_start_here/before_you_start_unix)
- [CORE] Enhanced the CLEARCMD command so that INDEX is optional and you can use a wildcard in the JOBNAME. [Details](../c_intro_userinterfaces/about_cftutil/managing_transfer_states/clearcmd_command)
- [CORE] Added the UCONF cg.metadata.agent parameter, which allows you to specify the Agent to use while connecting to Flow Manager in SaaS mode.
- [CORE] Added SAML signature support on Windows and HPUX-ia64.
- [CORE] Improved overall SFTP implementations so that there are now SecureTransport metadata extensions for Sentinel Monitoring, and an increased PART ID field of up to 64 characters.
- [CORE] Removed support for the EBICS protocol.
- [API] Added ability to manage hosts and nodes using REST APIs. [Details](http://apidocs.axway.com/swagger-ui/index.html?productname=transfercft&productversion=3.6&filename=transfercft-swagger-api.json)
- [API] PKI management for REST APIs and the UI. [Details](../c_intro_userinterfaces/web_copilot_ui/cftssl/pkicer)
- [API] Increased the default number of REST API server workers from 1 to 4.
- [GUI] Added a homepage to the Web-based UI.
- [GUI] Integrated the Swagger UI in the Web-based UI.
- [SECURITY] Added brute force protection. *Available on Unix, IBM i, HP NonStop, and Windows *[Details](../c_intro_userinterfaces/web_copilot_ui)
- [SECURITY] Added support for 4096-bit key for certificates generated during registration with {{< TransferCFT/suitevariablesCentralGovernanceName  >}} (see *Change private key length*). [Details](../governance_services_intro/register_cg)
- [SECURITY] Password encryption for silent file installations. *UNIX *[Details](../cft_intro_install/unix_install_start_here/before_you_start_unix/new_install_ux)*,* and *Windows* [Details](../cft_intro_install/windows_install_start_here/before_you_start_win/properties_file_win)
- [SECURITY] Fine Grain Access Control: you can use the NIDF parameter as a property of the TRANSFER resource.
- [HPNS] SFTP is now available on Transfer CFT HP NonStop exclusively for OSS files. [Details](../protocols_start_here/sftp_intro)
- [IBM i] Support passphrase of up to 32 characters for login connections with the Copilot server (UI, REST API).
- [IBM i] The cft_support now collects SBS information.
- [UNIX] Added ipcs -p command output while generating a cft_suppor archive.
- [z/OS] Added support for the aes256-ctr, aes192-ctr, aes128-ctr cipher suites with the SFTP protocol.
- [z/OS] Support for SAML 2.0 as an Identity Provides (IdP) with Transfer CFT installations enables Single Sign-On (SSO) for z/OS. Details in the *[*Installation and Operations Guide z/OS*](../cft_intro_install/about_this_document_zos)*
- [z/OS] Removed the dependency to Xerces XML library.
- [z/OS] Added the ability to specify the VSAM component suffix when installing. Details in the *[Installation and Operations Guide z/OS](../cft_intro_install/about_this_document_zos)*

 

****Transfer CFT 3.5****

- [z/OS] SFTP standardization means you can now use Transfer CFT z/OS to perform SFTP file transfers (HFS only). Details in the *[Transfer CFT z/OS Installation and Operations Guide](../cft_intro_install/about_this_document_zos)*
- [CORE] Support for SAML 2.0 as an Identity Provides (IdP) with Transfer CFT installations enables Single Sign-On (SSO) for Windows and Unix platforms. [Details](../c_intro_userinterfaces/web_copilot_ui/use_saml)
- [CORE] Added API Access Tokens for use in REST API bearer authentication. [Details](../app_integration_intro/using_apis/api_intro/api_authentication)
- [CORE] You can integrate an active/active Transfer CFT architecture with multiple Secure Relay Router Agents to provide secure data distribution in a high-availability scenario. [Details](../transport_security_start_here/sr_overview/cft_sr_conf_multinode)
- [CORE] Developed and added Transfer CFT features to the web browser user interface, including Transfer CFT sever management (check the status, start, stop, and restart). See the [User interface comparison]() table.
- [CORE] From the REST API you can perform Transfer CFT server administration (check the status, start, stop, and restart). Additionally, you can manage CFT, CFTIDF, and CFTXLATE objects. See the [Transfer CFT Swagger](http://apidocs.axway.com/swagger-ui/index.html?productname=transfercft&productversion=3.5&filename=transfercft-swagger-api.json) documentation for details.
- [CORE] You can now implement Transfer CFT in a Microsoft Azure environment. [Details](../datasheet#Virtuali)
- [CORE] You can integrate GlusterFS as storage for file transfers. [Details](../cft_intro_install/unix_install_start_here/before_you_start_unix/n_active_active/shared_file_prereq_ux#Standalo)
- [UNIX] Added parameters that allow you to configure the Transfer CFT `systemd` service when using Central Governance. [Details](../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/systemd_service_ux)
- [CORE] Implemented an automatic adding of the “cft service account” to the super user group. [Details](../internal_a_m_start_here/fm_access_management)
- [CORE] Added a UCONF parameter that allows you to blacklist certain characters in a processing script. [Details](../c_intro_userinterfaces/command_summary/symbolic_variables)
- [CORE] You can now use PKIUTIL PKIEXT to export a key in the PKCS\#8 format. [Details](../transport_security_start_here/certificates/pkiutil_cli_intro/pkiext)

****Transfer CFT 3.4****

- [CORE] Added support for FACTION=DELETE when sending a group of files in homogeneous mode.
- [CORE] Added support for Microsoft Azure VM instances.
- [CORE] Added SFTP: Support the use of the SFTP protocol for file transfers. [Details](../protocols_start_here/sftp_intro)
- [CORE] Amazon S3 can now be integrated with you SFTP flows. [Details](../protocols_start_here/sftp_intro#Using)
- [Windows] Added support for Amazon S3 capabilities on Windows 64 bit. [Details](../app_integration_intro/amazon_s3)
- [CORE] Added REST API for configuration management. [Details](../app_integration_intro/using_apis/api_intro)
- [CORE] The new Graphical User Interface (Web-UI) supports configuration management. [Details](../c_intro_userinterfaces/web_copilot_ui/conf_intro)
- [Linux] Docker support; delivery of Docker image and Docker material for building your own Transfer CFT Docker image. [Details](../cft_intro_install/contanier_intro/install_container)
- [CORE] New lightweight installer. Refer to the UNIX and Windows *Transfer CFT 3.4 Installation and Operation Guides*.
- [CORE] Folder monitoring run-time customization with support for a new symbolic variable syntax. [Details](../app_integration_intro/intro_folder_monitor/configure_folder_monitoring#Customiz)
- [CORE] Added a PASSWORD for the PKIEXT command, which enables you to export the user certificate and key in PKCS12 format. [Details](../transport_security_start_here/certificates/pkiutil_cli_intro/pkiext)
- [CORE] You can now use a force retry for a transfer in the YR state using the start command.
- [WIN] Added support for TrustedFile on Windows 64-bit.
- [CORE] Updated some default values for the CFTTCP, CFTNET, CFTPART and CFTPARM commands. [Details]()
- [CORE] Added parameter to define the local network interface for the outbound PassPort AM connection. [Details](../internal_a_m_start_here/about_passport_am/configure_passport_am)
- [AM] You can now use Central Governance as a form of access management. [Details](../internal_a_m_start_here/fm_access_management)
- [Linux/Windows] Google Cloud Platform support.

****Transfer CFT 3.3.2****

- [IBM i] Added support for folder monitoring on the native partition. SP2 [Details](../app_integration_intro/intro_folder_monitor/folder_ibm_i_native)
- [z/OS] Use SAF resources for internal access management mode. SP2 [Details](https://docs.axway.com/bundle/TransferCFT_38_InstallationGuide_mvs_en_PDF/resource/TransferCFT_InstallationGuide_mvs_en.pdf)
- [z/OS] The SSL certificate used by the Copilot server can be stored in a PDS/PDSE. SP2
- [CORE] Added database caching to reduce files I/O while accessing Transfer CFT parameters. SP2 [Details](../about_multinode/database_tuning)
- [CORE] Strengthening of SSL client authentication for business flow (CFTSSL VERIFY=ENFORCED). SP2 [Details](../c_intro_userinterfaces/command_summary/parameter_intro/verify)
- [CORE] Added support of dual authentication (password plus a private key) for the SFTP protocol. SP2 Details
- [CORE] The command CFTUTIL ABOUT can now list all license keys set in the license key file. SP2 [Details](../c_intro_userinterfaces/about_cftutil/about_command)
- [CORE] You can now import a PKCS\#12 certificate with multiple intermediate certificates in the chain. SP2 [Details](../c_intro_userinterfaces/command_summary/parameter_intro/inum)
- [CORE] You may use only one key for multi-node installations. SP2 Details
- [CORE] Added command to extract the access management cache file. SP2 [Details](../troubleshoot_intro/admin_troubleshooting_server/admin_troubleshooting_runtime/extract_am_cache)
- [CORE] Added support for arguments in the exec parameter and direct calls to scripts. *Windows and Unix* SP1 [Details](../concepts/about_transfer_processing/proc_commands)
- [CORE] Added new CFTUTIL command CHECK to verify parameter coherence. SP1 [Details](../c_intro_userinterfaces/about_cftutil/check_command)
- [CORE] Added new parameters XLATE, OCHARSET, and ICHARSET for the COPYFILE command. SP1 [Details](../admin_intro/admin_commands_intro/copyfile_command)
- [CORE] Added new parameters, TYPE and ID, to the PKIEXT command. SP1 [Details](../transport_security_start_here/certificates/pkiutil_cli_intro/pkiext)
- [CORE] You can now use PKIUTIL PKIKEY or PKIKUTIL PKICER to handle encrypted PEM private keys. SP1 [Details]()
- [CORE] Added keyboard shortcuts to improve accessibility in the Transfer CFT UI. SP1 [Details](../c_intro_userinterfaces/web_copilot_ui)
- [CORE] In the SWAITCAT command, added support for the T value in the PHASES parameter, and in the SEND and RECV commands added support for the T value in the WPHASES parameter. SP1 PHASES [Details](../c_intro_userinterfaces/command_summary/parameter_intro/phases), WPHASES [Details]()
- [CORE] Added the requester JOBNAME in the `CFTR12I` message for SEND and RECEIVE commands (log format V24). Added requester JOBNAME in the `CFTS20I `message (when the communication file row number is deleted). SP1 CFTR12I [Details](../troubleshoot_intro/messages_and_error_codes_start_here/cftr_messages#CFTR12I), CFTS20I [Details](../troubleshoot_intro/messages_and_error_codes_start_here/cfts_messages#CFTS20I)
- [CORE] Added new TLS1.2 cipher suites for the EBICS protocol. SP1 UI Details, CLI Details
- [CORE] New Transfer CFT Graphical User Interface (replaces the Copilot user interface). [Details](../c_intro_userinterfaces/web_copilot_ui)
- [CORE] Added custom ciphering key to protect sensitive data. [Details](../transport_security_start_here/cipher_key)
- [LINUX] Added support for Amazon S3 capabilities. [Details](../app_integration_intro/amazon_s3)
- [CORE] Added support for SSH File Transfer Protocol (SFTP) to transfer files using Transfer CFT as a server as well as a client. *Exclusively for Windows and Unix (see the SFTP introduction for a detailed list) as a technical preview*. [Details](../protocols_start_here/sftp_intro)
- [LINUX] Added support for Amazon Web Services Elastic File System (EFS). [Details]()
- [CORE] Added fine grain access control to the SERVICE:UI resource. Refer to the *Transfer CFT 3.3.2 Security Guide* *&gt; *FGAC privileges** for more information.
- [z/OS] Added support for password phrase.
- [UNIX] Added the Transfer CFT utilities Bash auto-completion command feature. [Details](../c_intro_userinterfaces/about_cftutil/autocomplete)
- [CORE] Added support for a range of internal communication ports between nodes in a multi-node installation (cft.multi_node.listen_port_range). [Details](../admin_intro/uconf/uconf_directory)
- [z/OS] Added an EXECINFO catalog field, which contains the JOBID:JOBNAME of the last processing procedure executed.
- [CORE] Added NIDT attribute as catalog selection criteria. [Details]()

****{{< TransferCFT/axwayvariablesComponentLongName  >}} 3.2.4****

- [z/OS] Added support to connect to Copilot as a system user or as a Central Governance user (uconf:copilot.misc.createprocessasuser). Refer to the *Transfer CFT z/OS Installation and Operations Guide*.
- [z/OS] Added support for MSA4 extensions and Galois Counter Mode: GCM (AES-[128&#124;256]).
- [CORE] You can now customize the SSL certificate distinguished name (DN) generated during Central Governance registration. SP1 Details
- [CORE] The Copilot server automatically uses the SSL certificate, generated during {{< TransferCFT/suitevariablesCentralGovernanceName  >}} registration, for SSL connections. SP1 [Details](../admin_intro/manage_copilot)
- [SSL] Added support for synchronous COM SSL. SP1 [Details]()
- [API] Added timeout feature for REST API requests. SP1 [Details](../app_integration_intro/using_apis/api_intro/api_commands)
- [CORE] Updated information for the Sentinel connector. SP1 [Details](../using_sentinel/trk_parms)
- [CORE] Enhanced the MQUERY component information. SP1 [Details](../admin_intro/admin_commands_intro/querying_a_component_)
- [CORE] Added support for Server Name Identification (SNI), a TLS protocol extension. SP1 [Details](../transport_security_start_here/manage_ssl_tls_versions)
- [CORE] Added REST API for transfer management (Unix/Windows). [Details](../app_integration_intro/using_apis/api_intro)
- [CORE] Added CFTFOLDER object for folder monitoring configuration. [Details](../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftfolder)
- [CORE] Enhanced folder monitoring with file-system event mode. [Details](../app_integration_intro/intro_folder_monitor)
- [CORE] Support of regular expressions for folder monitoring. [Details](../app_integration_intro/intro_folder_monitor/folder_customize)
- [CORE] Added processing throttling to limit the number of scripts launched in parallel. [Details](../concepts/about_transfer_processing/proc_commands)
- [UNIX] Support product updates using {{< TransferCFT/suitevariablesCentralGovernanceName  >}} when the system user control is enabled.
- [CORE] Support regular expressions for multi-pattern groups of files. [Details](../concepts/send_command/send_group_of_files_cl#Create)
- [CORE] Extended transcoding (FCHARSET/NCHARSET) support for stream text. [Details](../concepts/transfer_command_overview/using_transcoding/use_extended_character_sets)
- [IBM i] Added stream text support (FTYPE=J). [Details](../c_intro_userinterfaces/command_summary/parameter_intro/ftype)
- [CORE] Allow nfname (PI37) larger than 80 characters with the other PeSIT E products. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/nfname)
- [UNIX] Added support for GFS2 shared file system for multi-node architecture. [Details](../about_multinode)
- [CORE] Added a feature to optimize network resources, NRDP. [Details](../concepts/about_parallel_transfers/prevent_network_depletion)
- [CORE] Added ability to use the hexadecimal representation in FPAD and NPAD. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/fpad) and [Details](../c_intro_userinterfaces/command_summary/parameter_intro/npad)
- [CORE] Added post-transfer file renaming option. [Details](../app_integration_intro/spoolout)
- [z/OS] Added support for cft.server.exec_as_user. [Details](../internal_a_m_start_here/user_rights_overview/user_rights_file_rights#Set2)
- [AM] Enhanced the {{< TransferCFT/suitevariablesTransferCFTName  >}} to PassPort decoupling by adding a new process to update the persistent cache. [Details](../internal_a_m_start_here/about_passport_am/unconf_access_management)
- [CORE] Transfer CFT sends notifications to the Sentinel server only for errors. [Details](../using_sentinel/trk_parms)
- [CORE] Added HOST information to the message CFTH56I. [Details](../troubleshoot_intro/messages_and_error_codes_start_here/cfth_messages#CFTH56I)
- [CORE] Added support of relative bounds for date and time when using LISTLOG. [Details](../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/listlog)
- [CORE] Added session identifier to CFTT57I and CFTT58I log messages.
- [CORE] Update end-of-transfer EXIT with IDTU, PIDTU, RIDTU, GROUPID, IsRelay information.

****{{< TransferCFT/axwayvariablesComponentLongName  >}} 3.2.2****

- [SSL] Added UCONF parameters sentinel.xfb.use_ssl and sentinel.xfb.ca_cert_id to secure a connection between Transfer CFT and the Sentinel server. [Details](../admin_intro/uconf/uconf_directory)
- [SSL] Added support for TLS 1.1 and 1.2 [Details](../transport_security_start_here/manage_ssl_tls_versions), and additional cipher suites. [Details](../transport_security_start_here/configuring_transport_security_start_here/transport_security_cftssl)
- [SSL] Added support for Perfect Forward Secrecy. [Details](../transport_security_start_here/manage_cipher_suites)
- [EXIT] Added PKI exit functionality and sample for UNIX platforms. [Details](../transport_security_start_here/using_pki_exits_start_here)
- [CORE] Added log output for rotation (backup) files when using LISTLOG. [Details](../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/listlog)
- [CORE] Added new SERIALIZED parameter to filter the displayed transfer records. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/serialized)
- [CORE] Documented symbolic variable used to reconstitute a filename on reception. [Details](../c_intro_userinterfaces/command_summary/symbolic_variables#Example)
- [CORE] Added CFTUTIL autocomplete command feature for UNIX/Windows. [Details](../c_intro_userinterfaces/about_cftutil/autocomplete)
- [CORE] Added previous/next command feature for UNIX/Windows. [Details](../c_intro_userinterfaces/about_cftutil/command_recall)
- [CORE] Delivered a spool out sample. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/serialized)
- [IBM i] Process outputs (QPRINTS) are redirected to the following files in the production library: the CFTOUT file for Transfer CFT, and the COPOUT file for COPILOT. [Details](https://docs.axway.com/bundle/TransferCFT_38_InstallationGuide_os400_en_PDF/resource/TransferCFT_InstallationGuide_os400_en.pdf)
- [IBM i] Process joblogs (QPJOBLOG) are redirected to the following files in the production library: JLOG and JLOG2 files for Transfer CFT, and the COPJLOG and COPJLOG2 files for Copilot. [Details](https://docs.axway.com/bundle/TransferCFT_38_InstallationGuide_os400_en_PDF/resource/TransferCFT_InstallationGuide_os400_en.pdf)
- [CORE] Added support for the pre, post, and ack processing states to the Transfer JPI. [Details](../cft_intro_install/about_this_document_ibmi/using_apis/java_api)
- [CORE] Added new parameters [SY]() and [RY](), and UCONF parameters cft.purge.sy and cft.purge.ry, to automatically purge transfers in the Y (post-processing phase) state. [Details](../admin_intro/admin_monitoring_intro/housekeeping_catalog)
- [CORE] Added a new parameter [SERIAL](../c_intro_userinterfaces/command_summary/parameter_intro/serial) for the SEND/RECV commands. [Details](../app_integration_intro/transfer_serialization)
- [IBM i] Added the CFTSUPPORT command. [Details](https://docs.axway.com/bundle/TransferCFT_38_InstallationGuide_os400_en_PDF/resource/TransferCFT_InstallationGuide_os400_en.pdf)
- [CORE] Added new parameters [WPHASESTEPS]() and [WPHASE]() for the SEND/RECV commands.
- [CORE] Added a new parameter FILENOTFOUND for the CFTSEND/CFTRECV objects and the SEND/RECV commands. [Details]()
- [CORE] Added a new parameter FILENOTFOUND for the SEND and RECV commands. [Details]()
- [UNIX] Added the RENAME option to the FACTION parameter, which replaces the existing FNAME file after the transfer completes with the WFNAME. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/faction)
- [CORE] Dynamically add, remove, enable, or disable a logical folder to monitor in the Transfer CFT runtime. [Details](../app_integration_intro/intro_folder_monitor/folder_monitor_uconf#Modifying_existing_configuration)
- [GUI] Replaced the Copilot UI applet with a Java Web Start technology UI due to discontinued support for Java applets by web-browsers. Details
- [CORE] PKI exits available on all platforms.
- Added a reusable deployment package feature (new JCLs) for installing or applying Service Packs/patches to Transfer CFT in a z/OS environment. Details provided in the *Transfer CFT z/OS Installation and Operations Guide.*

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 3.1.3****

- [CORE] Added a new UCONF parameter cg.configuration_policy. SP4 [Details](../governance_services_intro/cg_register_overview)
- [CORE] Support for the NCHARSET and FCHARSET "//IGNORE" functionality on all platforms. SP4 [Details](../concepts/transfer_command_overview/using_transcoding/use_extended_character_sets#Using)
- [CORE] Added a UCONF parameter copilot.misc.server_inactivity_timeout for the server to manage client inactivity. SP3 [Details](../admin_intro/uconf/uconf_directory)
- [CORE] Added a UCONF parameter pesit.parameters.enable_encoding to define if PeSIT parameters should be encoded in UTF-8. UNIX/Windows SP1 [Details](../admin_intro/uconf/uconf_directory)
- [CORE] Added charset encoding support for messages being sent to the Sentinel server using the UCONF sentinel.trkmsgencoding parameter. SP1 [Details](../admin_intro/uconf/uconf_directory)
- [CORE] When using the folder monitoring feature, improved the file name filtering by adding a new UCONF parameter folder_monitoring.folders.&lt;logical_name&gt;.filter_type. SP1
- [CORE] Added policy support for EXECE. [Details](../concepts/about_transfer_processing/processing_exec_policy)
- [CORE] Added a new policy to manage the behavior when sending a group of files using an indirection file. [Details](../concepts/send_command/send_group_of_files_cl)
- [CORE] Use a shared disk as the data transfer medium.
- [CG] Certificate renewal. [Details](../governance_services_intro/cg_postregister)
- [CG] SP and patch deployment via {{< TransferCFT/PrimaryCGorUM  >}}. *Unix/Windows only*[Details](../governance_services_intro/cg_postregister)
- [CG] {{< TransferCFT/suitevariablesCentralGovernanceName  >}} support for additional platforms z/OS and IBM i. [Details](../governance_services_intro/governance_overview)
- [CORE] Added internal log rotation procedure. [Details](../admin_intro/admin_monitoring_intro/housekeeping_logs)
- [CORE] A new command PKIENTITY enables you enlarge the list of certificate authority IDs for each security configuration. [Details](../transport_security_start_here/certificates/pkiutil_cli_intro/pkientity)
- [CORE] Added a UCONF parameter to improve the file layer service reliability: cft.server.transfer.raise_error_when_exec_not_found.  [Details](../concepts/about_transfer_processing/processing_exec_policy)
- [SSL] Added parameter to disable SSL 3.0. [Details](../transport_security_start_here/manage_ssl_tls_versions)
- [CORE] Added the keyword _NONE_ to disable the execution of certain CFTPARM general configuration procedures. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/none)
- [CORE] Added a new FTYPE = J to support large records for text files. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/ftype)
- [IBM i] IASP support. Refer to the *Transfer CFT 3.1.3 Installation and Operations Guide*
- [z/OS] Folder monitoring, Secure Relay, working directory on z/OS systems. For feature details see 3.1.2 SP1
- [IBM i] Folder monitoring, Secure Relay, working directory on IBM i systems. For feature details see 3.1.2 SP1
- [OpenVMS]  Folder monitoring, working directory.  For feature details see 3.1.2 SP1

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 3.1.2****

- [CORE] Added Express Package functionality to generate pre-configured {{< TransferCFT/axwayvariablesComponentShortName  >}} installation packages. SP1 Details
- [CORE] Added working directory parameter. SP1 [Details](../c_intro_userinterfaces/command_summary/parameter_intro/workingdir)
- [CORE] Added keyword &HOME. SP1 [Details](../c_intro_userinterfaces/command_summary/parameter_intro/home)
- [WINDOWS] Homogeneous mode now keeps the directory tree structure as in UNIX. SP1
- [CORE] {{< TransferCFT/axwayvariablesCompanyName  >}} Central Governance integration. [Details](../overview_intro/c_cg_concepts)
- [CORE] Enabled folder monitoring. [Details](../app_integration_intro/intro_folder_monitor/folder_monitor_uconf)
- [SSL] {{< TransferCFT/axwayvariablesCompanyName  >}} Secure Relay integration. [Details](../transport_security_start_here/sr_overview)
- [COMS] Synchronous Communication Media in multi-node architecture. [Details](../about_multinode)
- [NET] Support for pTCP in a multi-node architecture. Backported 3.0.1 SP2 [Details](../concepts/transfer_command_overview/uconf_acceleration#uconf_ptcp)
- [AM] {{< TransferCFT/axwayvariablesCompanyName  >}} PassPort AM local cache. [Details](../internal_a_m_start_here/about_passport_am/unconf_access_management)
- [GOVERNANCE] Improved event generation. [Details](../using_sentinel/intro_sentinel/mapping_states)
- [GOVERNANCE] Send NIDF instead of COMMUT to Axway Sentinel. [Details](../using_sentinel/uconf_sentinel)
- [CORE] Added EXECRALL post-processing parameter to the receive command. Backported 3.0.1 SP4 and 2.7.1 SP8. [Details]()
- [CORE] Added requester mode transfer parameter maxduration as a transfer timer. [Details]()
- [CORE] Encrypt all passwords, previously some passwords were stored in clear.
- [CORE] Added PROT parameter to SEND/RECV. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/prot)
- [CORE] Enable SEND or RECV with no IDF. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/idf#IDF_send_recv)
- [CORE] Enable/disable the use of a default file identifier. [Details](../admin_intro/uconf/default_idf)
- [CORE] Added auto-expand catalog option. [UCONF](../admin_intro/uconf/uconf_parameters), [Details](../admin_intro/admin_monitoring_intro/auto_expand_catalog)
- [CORE] Filter Transfer CFT logs by absolute timestamp datetimemin and datetimemax. [Details](../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/listlog)
- [CORE] Web Services upload files feature. [Details](../cft_intro_install/about_this_document_ibmi/using_apis/about_web_services)

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 3.0.1****

- [DOC] Added UCONF directory listing. SP4 [Details](../admin_intro/uconf/uconf_directory)
- [NET] Support for a newer version of pTCP. ****Note****: See compatibility restrictions with previous versions of {{< TransferCFT/axwayvariablesComponentShortName  >}}. SP2 [Details](../concepts/transfer_command_overview/uconf_acceleration#uconf_ptcp)
- [AM] Enhanced system user control setting. SP2
- [UNIX] Improved service pack upgrade procedure when using system user control (CFTSU). SP2
- [CORE] Added padding/unpadding character for text files at the file and network level. SP1 [Details](../concepts/transfer_command_overview/padding)
- [CORE] Manage child transfers using the generic parent. SP1 Details
- [CORE] Added MAXTIME parameter to START command. SP1 [Details](../c_intro_userinterfaces/command_summary/parameter_intro/maxtime)
- [COPILOT] Added keep-alive parameter for client connections. SP1 [Details](../admin_intro/uconf/uconf_copilot)

<!-- -->

- [CORE] Multi-node architecture (active/active high availability, horizontal scalability, traffic management). [Details](../about_multinode)
    -   [CORE] Multi-node LISTLOG. [Details](../about_multinode/multi_node_commands#Multi%20N%20Listlog)
    -   [CORE] Multi-node DISPLAY and LISTCAT. [Details](../about_multinode/multi_node_commands#Multi%20N%20Display)
    -   [CORE] Multi-node LISTNODE. [Details](../about_multinode/multi_node_commands#Multi%20N%20Listnode)
    -   [CORE] New commands to enable/disable nodes. [Details](../about_multinode/multi_node_commands#Multi%20N%20enable%20disable)
    -   [CORE] Node manger and connection dispatcher in Copilot. [Details](../about_multinode)
- [CORE] File service layer
    -   [CORE] Preprocessing. [Details](../concepts/phase_and_phasestep/preprocessing)
    -   [CORE] Processing generic groups of files. Details
    -   [NET] S/MIME and PGP with Trusted File. [Detail](../transport_security_start_here/tf_overview_cft)
- [CORE] Added bandwidth control. [Details](../concepts/c_bandwidth)
- [COM] Synchronous COM authentication support. Backported, see 2.7.1 and 2.6.4. [Details](../c_intro_userinterfaces/about_cftutil/control_remote_users_synch_com)
- [AM] Internal Access Management added (uconf:am.type=internal). Backported, see 2.7.1 and 2.6.4. [Details](../internal_a_m_start_here/uconf_internal_am)
- [CORE] For generic transfers, parent state reflects child transfer state. [Details](../concepts/phase_and_phasestep)
- [CORE] Integrated SWAITCAT. Details
- [SSL] Enable PassPort PS SSL connectivity. *Discontinued in Transfer CFT 3.10.*
- [SSL] Transparent SSL/TLS (compatibility). Backported, see 2.7.1 SP3. [Details](../transport_security_start_here/configuring_transport_security_start_here/transport_security_cftssl)
- [CORE] Purge retention period. [Details](../admin_intro/admin_commands_intro/purge_catalog)
- [CORE] Unified configuration scheduling. [Details](../admin_intro/uconf/uconf_scheduler)
- [CORE] Parent IDTU. The parent transfer states now immediately reflect the children transfer states. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/pidtu)
- [CORE] Rotate script to create additional log files. [Details](../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/rotate_script)
- [CORE] Executable file to retrieve system details CFTTELL. [Details](../admin_intro/admin_commands_intro/cfttell)
- [GUI] Updated Wizards. Details
- [GUI] CFT_support is generated directly from Copilot. [Details](../cft_intro_install/unix_install_start_here/troubleshoot_registration/support_tools)
- [NET] Support for NFSv4. [Details]()
- [CORE] Added padding/unpadding character for text files at the file and network level. SP1 [Details](../concepts/transfer_command_overview/padding)
- [CORE] Manage child transfers using the generic parent. SP1 Details
- [CORE] Added MAXTIME parameter to START command. SP1 [Details](../c_intro_userinterfaces/command_summary/parameter_intro/maxtime)
- [COPILOT] Added keep-alive parameter for client connections. SP1 [Details](../admin_intro/uconf/uconf_copilot)
- [NET] Support for a newer version of pTCP. ****Note****: See compatibility restrictions with previous versions of Transfer CFT. SP2 [Details](../admin_intro/uconf/uconf_protocols_and_networks)

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 2.7.1****

- [NET] Support for more recent version of pTCP. ****Note****: Compatibility restrictions with lower Transfer CFT versions. SP6 [Details](../admin_intro/uconf/uconf_protocols_and_networks)
- [COMS] COMS authentication support. Backported in 2.7.1 SP3.
- [AM] Internal AM (uconf:am.type=internal). Backported in 2.7.1 SP3.
- [PROTOCOL] EBICS proxy support. Backported in SP4. Details
- [NET] Add IPv6 support (`uconf:cft.ipv6.*`). [UCONF](../admin_intro/uconf/uconf_protocols_and_networks), [Details](../protocols_start_here/ipv6)
- [NET] Transfer acceleration using UDT, pTCP (`uconf:acceleration.*`). [Details](../concepts/transfer_command_overview/uconf_acceleration)
- [NET] Add SOCKS5 support (CFTNET). [Details](../protocols_start_here/ipv6/use_proxy_and_socks_protocol)
- [CORE] Duplicate transfer management. [DUPLICAT](../c_intro_userinterfaces/command_summary/parameter_intro/duplicat), [DACTION](../c_intro_userinterfaces/command_summary/parameter_intro/daction)
- [CORE] Password management (SEND/RECV/CFTSEND/CFTRECV). [Details](../transport_security_start_here/password_management), [SPASSWD](../c_intro_userinterfaces/command_summary/parameter_intro/spasswd), [RPASSWD](../c_intro_userinterfaces/command_summary/parameter_intro/rpassw)
- [CORE] Delete file when purging/deleting the catalog record transfers. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/fdelete)
- [CORE] Reload the dynamic unified configuration parameters (`CFTUTIL  RECONFIG TYPE=UCONF`). [Details](../admin_intro/admin_commands_intro/reconfig)
- [CORE] Dynamically increase catalog size (`CFTUTIL RECONFIG TYPE=CAT,RECNB=xxx`). [Details](../admin_intro/admin_commands_intro/reconfig)
- [CORE] Dynamic catalog purge. (`uconf:cft.purge.r*`&#124;`uconf:cft.purge.s*`) [Details](../admin_intro/admin_commands_intro/purge_catalog), [UCONF](../admin_intro/uconf/uconf_parameters)
- [CORE] Display key information in `CFTUTIL ABOUT`. [Details](../c_intro_userinterfaces/about_cftutil/about_command)
- [CORE] Customizable network sessions (`uconf:cft.server.max_session`). [UCONF](../admin_intro/uconf/uconf_parameters)
- [CORE] Adaptive memory consumption based on the MAXTRANS parameter and `uconf:cft.server.max_session`.
- [CORE] Password authentication to control remote users using synchronous communication media. [Details](../c_intro_userinterfaces/about_cftutil/control_remote_users_synch_com), [UCONF](../internal_a_m_start_here/about_passport_am/unconf_access_management)
- [CORE] Force transferring a group of files to occur exclusively in heterogeneous mode. [Details](../concepts/send_command/send_group_of_files_cl), [UCONF](../admin_intro/uconf/uconf_heterogeneous_mode)
- [UNIX] Performance improvement for end-of-transfer submission procedure.
- [UNIX] Add option to separate the file name from the file extension. [UCONF](../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/uconf_unix), [Details](../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/suffix_management)
- [SENTINEL] Integrated Sentinel Heartbeat (`uconf:sentinel.heartbeat.*`). [UCONF](../using_sentinel/uconf_sentinel)
- [SENTINEL] Sentinel transfer progress notifications. (`uconf:sentinel.xfb.transfer_progress_period`).  [UCONF](../using_sentinel/uconf_sentinel)
- [NAVIGATOR] Transfer CFT Navigator self-registration (`uconf:navigator.*`). UCONF
- [GOVERNANCE] Start/stop scripts. [UNIX](), [Windows](../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows)
- [GOVERNANCE] SSA (Synchrony Support Assistant) support. [Details](../cft_intro_install/unix_install_start_here/troubleshoot_registration/support_tools)
- [ADMIN] Integrated restart management (SHUT RESTART=YES). [Details](../admin_intro/admin_commands_intro/shut_command)
- [ADMIN] Display internal statistics (MQUERY OBJECT=STATS). [Details](../admin_intro/admin_commands_intro/querying_a_component_)
- [ADMIN] Add raw output from probes (MQUERY OBJECT=PROBE,CONTENT=RAW). [Details](../admin_intro/admin_commands_intro/querying_a_component_)
- [CMDLINE] Enhanced Unix command line tools usability (readline/history integration). [Details](../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/uconf_unix)
- [JPI] Add SOCKS5 support.
- [JPI] Display acknowledgement status (ACK and NACK).
- [JPI] Add IDTU criteria for transfer selection.
- [CORE] Internal access management.

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 2.7.0****

- [CORE] Added EBICS protocol. Details
- [CORE] Added new start-up modes for the Catalog purge ([uconf](../admin_intro/uconf/uconf_parameters):cft.purge.\*).
- [CORE] Enlarged [USERID](../c_intro_userinterfaces/command_summary/parameter_intro/userid) to 32.
- [CORE] Added Windows domain user functionality (DOMAIN\\USER). [Details](../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows/user_rights_and_interface_win)
- [CORE] Enlarged CFTTCP [HOST](../c_intro_userinterfaces/command_summary/parameter_intro/host) field to 512.
- [CORE] Increased the number of parallel transfers to 1000. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/maxtrans)
- [CORE] NACK: Added Partner/PeSIT protocol option. [Details](../concepts/send_command/transfers_neg_ack_pesit)
- [CORE] File actions executed for system users ([Unix](../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/t_adding_system_user_unix)/[Windows](../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows/add_system_user_windows)).
- [CORE] Post-transfer procedure executed for system users ([Unix](../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/t_adding_system_user_unix)/[Windows](../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows/add_system_user_windows)).
- [SSL] Improved error messages.
- [SSL] CFTSSL CIPHLIST 53 (AES-256 and SHA1) supported. [Details](../transport_security_start_here/configuring_transport_security_start_here)
- [SSL] CFTSSL CIPHLIST 59 (None and SHA256) supported. [Details](../transport_security_start_here/configuring_transport_security_start_here)
- [SSL] CFTSSL CIPHLIST 60 (AES-128 and SHA256) supported. [Details](../transport_security_start_here/configuring_transport_security_start_here)
- [SSL] CFTSSL CIPHLIST 61 (AES-256 and SHA256) supported. [Details](../transport_security_start_here/configuring_transport_security_start_here)
- [SSL] Added SHA-224, SHA-256, SHA-384, and SHA-512 signature hash algorithm support for RSA certificate.
- [SSL] Increased X.509 certificate support to 4 KB in size. [Details](../transport_security_start_here/certificates/certificate_concepts)
- [SSL] Self-signed certificate support for transport.[Details](../transport_security_start_here/configuring_transport_security_start_here)
- [SSL] Transparent certificate import (DER/PEM). [Details](../transport_security_start_here/configuring_transport_security_start_here)
- [AM] Added an attribute to SUSER/RUSER/SAPPL/RAPPL/NFNAME for a transfer.
- [CFTUTIL] CFTEXT [TYPE=UCONF] extracts the unified configuration. [Details](../c_intro_userinterfaces/about_cftutil/configuring_cft_start_here/cftext_command)
- [CFTUTIL] CFTTCOMS: extended the number of parallel connections [uconf](../admin_intro/uconf/uconf_parameters):cft.server.cftcoms.max_connection.
- [CFTUTIL] Unified configuration reloads CFTUTIL RECONFIG type=UCONF. [Details](../admin_intro/admin_commands_intro/reconfig)
- [GUI] View all Windows drives in the Transfer CFT GUI.Details
- [GUI] PassPort Access Management rights view. Details

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 2.6.4****

- [AM] Access Management Exit ([uconf](../admin_intro/uconf/uconf_parameters):am.type=exit).
- [AM] (z/OS) Access Management through RACF. Details
- [GUI] (z/OS) Integrated change password. Backported to 2.6.3_SP2. Details
- [GUI] Added configurable starting filters for Log and Catalog ([uconf](../admin_intro/uconf/uconf_parameters):copilot.startup.\*).
- [GUI] Added Catalog paging ([uconf](../admin_intro/uconf/uconf_parameters):copilot.catalog.paging=Yes).
- [GUI/SSL] Restricted GUI server access to SSL ([uconf](../admin_intro/uconf/uconf_parameters):copilot.http.onlyssl=yes).
- [EXEC] SUBF option of [EXECSUB](../c_intro_userinterfaces/command_summary/parameter_intro/execsub) now allows executing end-of-transfer procedures only on non-generic transfers.
- [EXEC] [EXECSUBA](../c_intro_userinterfaces/command_summary/parameter_intro/execsuba) (SUBF, LIST, FILE) parameter allows the user to choose when to submit an acknowledgement procedure on generic requests.
- [CFTUTIL/API] SWAITCAT IDA=&IDTU allows a wait period at the end of a generic request. [Details](../c_intro_userinterfaces/about_cftutil/managing_transfer_states/swaitcat_concepts)
- [CFTUTIL/API] SWAITCAT can now wait for the termination of a transfer in the STATE=HOLD with DIAGI=0. [Details](../c_intro_userinterfaces/about_cftutil/managing_transfer_states/swaitcat_concepts)
- [CFTUTIL] [LISTUCONF](../admin_intro/uconf/uconf_w_cftutil) CONTENT=EXTRACT, [FOUT](../c_intro_userinterfaces/command_summary/parameter_intro/fout)=out now extracts the unified configuration.
- [CORE] [RUSER](../c_intro_userinterfaces/command_summary/parameter_intro/ruser)/[SUSER](../c_intro_userinterfaces/command_summary/parameter_intro/suser) selection speedup ([uconf](../admin_intro/uconf/uconf_parameters):cft.cftcat.enable_user_quick_search=Yes).
- [[API](../cft_intro_install/about_this_document_zos/using_apis)] Added new methods in ipcai2_catalog_selection: sortby, skip, count.
- [SENTINEL] Added the ****Fill RecordFormat**** field. Details
- [SENTINEL] Added ****Fill ApplicationName**** field with [uconf](../admin_intro/uconf/uconf_parameters):sentinel.trkident. Details
- [SYST] Added sco-x86-32 support. Refer to the *Synchrony Supported Platforms Guide*.
- [SYST] Added uxw-x86-32 support. Refer to the *Synchrony Supported Platforms Guide*.
- [COMS] COMS authentication support (Backported 2.6.4.)
- [AM] Internal AM (uconf:am.type=internal) (Backported 2.6.4.)

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 2.6.3****

- [SSL] Added FIPS Compliant Algorithms ([uconf](../admin_intro/uconf/uconf_parameters):cft.fips.enable=Yes).
- [CORE] Added PRESTO link protocol support (deprecated after this version).
- [SYST] Added win-x86-64 support. Refer to the *Synchrony Supported Platforms Guide*.
- [SYST] Added irix-mips-64 support. Refer to the *Synchrony Supported Platforms Guide*.
- [SYST] Added tru64-alpha-64 support. Refer to the*Synchrony Supported Platforms Guide*.
- [SSL] Added RSA 4096-bit key support. [Details](../transport_security_start_here/configuring_transport_security_start_here)
- [FILE] (z/OS) Added z/OS 1.7 file extension support. Details
- [PERF] (z/OS) SHARECAT=YES allows the catalog to share between Transfer CFT and the GUI server. Details
- [CORE] (z/OS) Increased the number of parallel transfers from a limit of 240 to 500. Details
- [TCP] (z/OS) The TCP layer now uses 64-bit buffers. Details
- [SSL] (z/OS) CFTSSL sub-tasks have increased from 16 to 64 tasks. Details
- [SSL] (z/OS) CFTSSL CIPHLIST 53 (AES-256 & SHA1) supported on IBM Z/10. CFTSSL CIPHLIST 60 (AES-128 & SHA256) supported on IBM Z/9-109 and Z/10.
- [SSL] (z/OS) RACF PKI certificate management using the cryptographic Express2 coprocessor. Details
- [SSL] (z/OS) Hardware cryptography uses DES, AES, SHA on all z/series models across CPACF. Details
- [SSL] Authenticated connection to PassPort PKI Server ([uconf](../admin_intro/uconf/uconf_parameters):pki.passport.login, uconf:pki.passport.password). Details
- [GUI] Added dynamic filters.
- [GUI] Improved handling of encoding when viewing and editing a file.
- [GUI] Improved feedback when stopping/starting Transfer CFT server. Details
- [GUI] Added direct Transfer CFT server restart function.Details
- [GUI] Added CFTAPPL object handling. Details
- [LOG] Added custom filters for LOG output ([uconf](../admin_intro/uconf/uconf_parameters):cft.server.log.exclude_filters).[Details](../admin_intro/admin_monitoring_intro/housekeeping_logs)
- [CORE] (Windows) Added Systray management. Details
- [AM] (Unix/Windows) Connections to PassPort AM can now be secured ([uconf](../admin_intro/uconf/uconf_parameters):am.passport.use_ssl=Yes, uconf:am.passport.ca_cert). [Details]()
- [PACK] (Unix/Windows) Added $CFTDIRRUNTIME/profile.d for custom environment.
- [MIGR] (Unix/Windows) Added an integrated migration at installation. See the *Synchrony Platform Installation Guide*.

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 2.6.2****

- [AM] (Unix/Windows) Added PassPort Access Management. [Details](../internal_a_m_start_here/about_passport_am)
- [CORE] Added synchronous control of transfers ([SWAITCAT](../c_intro_userinterfaces/about_cftutil/managing_transfer_states/swaitcat_concepts)).
- [CORE] Extended charset support ([FCHARSET](../c_intro_userinterfaces/command_summary/parameter_intro/fcharset), [NCHARSET](../c_intro_userinterfaces/command_summary/parameter_intro/ncharset)) such as UTF-8, UTF-16, etc.
- [GUI] Allow GUI customization through plugin.xml handling ([uconf](../admin_intro/uconf/uconf_parameters):copilot.batches.XmlDefinition)
- [CORE] Added end-to-end negative acknowledgment function (NACK).
- [CORE] Added severity control to the WLOG command (WLOG [SEVERITY](../c_intro_userinterfaces/command_summary/parameter_intro/severity)).
- [CFTUTIL] Added RUSER/SUSER filtering in LISTCAT, DISPLAY command. [Details](../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/display_command)
- [GUI] Rootdirs can now contain %COPUSER% (Current userid) and environment variables (for example %HOME%, %USERPROFILE%).
- [COMPOSER] (Unix/Windows) Integrated Composer Connector ([uconf](../admin_intro/uconf/uconf_parameters):composer.\*).Details
- [SYST] Added aix-power-64 support. See the *Synchrony Supported Platforms Guide*.
- [SYST] Added hpux-parisc-64 support. See the *Synchrony Supported Platforms Guide*.
- [SYST] Added linux-x86-64 support. See the *Synchrony Supported Platforms Guide*.
- [SYST] Added linux-s390-64 support. See the *Synchrony Supported Platforms Guide*.
- [SYST] Added linux-s390-64 support. See the *Synchrony Supported Platforms Guide*.
- [SYST] Added sun-sparc-64 support. See the *Synchrony Supported Platforms Guide*.
- [SYST] Added sun-x86-64 support. See the *Synchrony Supported Platforms Guide*.
- [GUI] Added alias management ([uconf](../admin_intro/uconf/uconf_parameters):copilot.http.aliases).
- [SYST] (Unix) Added system user support ([uconf](../admin_intro/uconf/uconf_parameters):copilot.misc.CreateProcessAsUser=yes). Backported in 2.4.1_SP8, and 2.5.1_SP3.
- [SYST] (Windows) Manual installation of Service Mode using `cscript /nologo cftsrvin.vbs`. [Details](../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows)
- [GUI] Added an option to controls and limit the number of rows to be uploaded to the GUI for the Catalog and Log ([uconf](../admin_intro/uconf/uconf_parameters):copilot.cft.maxcatrows, uconf:copilot.cft.maxlogrows).
- [CORE] Created a new substitution syntax for default values (&(=DEFAULT)VAR, for example EXECSE=&(=&IDF)COMMENT). Backported in 2.4.1_SP5, 2.5.1.
- [CORE] CFTPART CTRLPART=[SPART](../c_intro_userinterfaces/command_summary/parameter_intro/spart)/[RPART](../c_intro_userinterfaces/command_summary/parameter_intro/rpart)/ALL/IGNORE
- [SYST] Added the capacity to retrieve subdirectories (uconf:cft.dirdepth=Yes). Backported in 2.4.1, 2.5.1. [Details](../admin_intro/uconf/uconf_parameters)
- [CFTUTIL] Added erase capability (CFTFILE MODE=ERASE). Backported in 2.4.1, 2.5.1. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/mode)
- [CFTUTIL] Display the Transfer CFT files that are used (PARM, PART, CAT, COM). Backported in 2.4.1, 2.5.1.
- [CORE] Extended the size of [NFNAME](../c_intro_userinterfaces/command_summary/parameter_intro/nfname) (PI37) between 2 Transfer CFTs to 512 characters. Backported in 2.4.1, 2.5.1.
- [SSL] Extended to 255 characters CFTSSL [ROOTCID](../c_intro_userinterfaces/command_summary/parameter_intro/rootcid) and [USERCID](../c_intro_userinterfaces/command_summary/parameter_intro/usercid) fields. Backported in 2.4.1, 2.5.1.

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 2.5.1****

- [CORE] Unified the configuration (UCONF) utility. [Details](../admin_intro/uconf)
- [SYST] Added sun-x86-32 support. See the *Synchrony Supported Platforms Guide*.
- [SYST] Added win-ia64-64 support.See the *Synchrony Supported Platforms Guide*.
- [CORE] Enabled Copilot configuration using the Unified Configuration ([uconf](../admin_intro/uconf/uconf_parameters):copilot.\*).
- [CORE] Enabled Sentinel configuration using the Unified Configuration ([uconf](../admin_intro/uconf/uconf_parameters):copilot.\*).
- [CORE] Enabled the IUI configuration using the Unified Configuration([uconf](../admin_intro/uconf/uconf_parameters):copilot.cft.com).
- [SSL] Added PassPort PKI server support ([uconf](../admin_intro/uconf/uconf_parameters): pki.type=passport, uconf:pki.passport.\*
- [CORE] Added job scheduling capability using CFTCRON, CFTPARM CRONTAB= feature.
- [CORE] Added ACT/INACT type=CRON.
- [CORE] Added ability to audit configuration modifications through Sentinel ([uconf](../admin_intro/uconf/uconf_parameters):sentinel.xfb.audit=Yes). [Details](../c_intro_userinterfaces/about_cftutil/auditing_config_changes)
- [SSL] Extended the [DNUSER](../c_intro_userinterfaces/command_summary/parameter_intro/dnuser) syntax.
- [TCP] Forced outgoing TCP/IP interface using CFTNET HOST=\*DEST or CFTNET [SRCHOST](../c_intro_userinterfaces/command_summary/parameter_intro/srchost)=DEST.
- [TCP] Created an outgoing port selection range using the CFTNET parameter [SRCPORTS](../c_intro_userinterfaces/command_summary/parameter_intro/srcports)=(range1,range2,...) and xSEND/xRECV [NETBAND](../c_intro_userinterfaces/command_summary/parameter_intro/netband)=0..n.
- [GUI] Improved access control through start page definition (index.html / admin.html). (*Deprecated*.)
- [CORE] Added configurable alert threshold for the Catalog (CFTCAT [TLVWARN](../c_intro_userinterfaces/command_summary/parameter_intro/tlvwarn)=,[TLVWEXEC](../c_intro_userinterfaces/command_summary/parameter_intro/tlvwexec), [TLVWRATE](../c_intro_userinterfaces/command_summary/parameter_intro/tlvwrate)=, [TLVCLEAR](../c_intro_userinterfaces/command_summary/parameter_intro/tlvclear)=, [TLVCEXEC](../c_intro_userinterfaces/command_summary/parameter_intro/tlvcexec)=).
- [CORE] Added controls for file attributes (CFTRECV [FCHECK](../c_intro_userinterfaces/command_summary/parameter_intro/fcheck)).
- [CORE] Enabled dynamic activation/deactivation of Sentinel tracking CFTUTIL [ACT](../c_intro_userinterfaces/about_cftutil/reactivate_an_object_cl) / [INACT](../c_intro_userinterfaces/about_cftutil/reactivate_an_object_cl/inact_command) type=[TRK](../c_intro_userinterfaces/command_summary/parameter_intro/trk).
- [TCP] A protocol can be defined as Requester only protocol, without defining the SAP.
- [CORE] Added new symbolic variables (&CFTEVENT, &SYSDAY, &CFTNAME). [Details](../c_intro_userinterfaces/command_summary/symbolic_variables)
- [CORE] Trace only on a specific partner.
- [CORE] Increased PeSIT message length from 512 to 4096.
- [SSL] Added verify x509 extension KEYEXT=[VERIFY](../c_intro_userinterfaces/command_summary/parameter_intro/verify).
- [PKIUTIL] Added extraction capability (PKIEXT) to PKIUTIL. Backported in 2.4.1, V2.5.1.

****{{< TransferCFT/axwayvariablesComponentShortName  >}} 2.4.1****

- [GUI] New Copilot applet GUI. Details
- [GUI] Integrated Web Services for Transfer CFT. [Details](../cft_intro_install/about_this_document_ibmi/using_apis/about_web_services)
- [GUI] Integrated basic HTTP server.
- [CORE] (Windows) ASCII file handling (X'1A') (xSEND/xRECV [FTYPE](../c_intro_userinterfaces/command_summary/parameter_intro/ftype)=F).
- [COMS] %_CAT_IDTU% %_CAT_IDT%.
- [CORE] Added symbolic variable &TRTYP (FILE, MESSAGE, REPLY). [Details](../c_intro_userinterfaces/command_summary/symbolic_variables)
- [CORE] DIAGI = 160 (RCVALLER=STOP, RCVALLER=CONTINUE).
- [CFTUTIL] Added the DISPLAY command. [Details](../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/display_command)
- [API] New [API2](../cft_intro_install/about_this_document_ibmi/using_apis/using_cft_services_in_c/t_capi2).
- [CORE] Extended the filenames to 512 characters. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/fname)
- [CORE] Extended identifiers to 32 characters. [Details](../c_intro_userinterfaces/command_summary/parameter_intro/id)
- [CORE] Allowed hiding private data in PI99 (CFTPROT [HIDE99](../c_intro_userinterfaces/command_summary/parameter_intro/hide99)=YES).
- [CORE] Added control for an initial sender's Identity (CFTPART CTRLPART=YES).
- [CORE] Added new V24 LOG Format (CFTLOG [FORMAT](../c_intro_userinterfaces/command_summary/parameter_intro/format)=V24).
- [CORE] Added new V24 ACCNT Format (CFTACCNT [FORMAT](../c_intro_userinterfaces/command_summary/parameter_intro/format)=V24).
- [CORE] Added new V24 EXIT Format (CFTEXIT [FORMAT](../c_intro_userinterfaces/command_summary/parameter_intro/format)=V24).
- [CORE] Extended CFTEXIT [PROG](../c_intro_userinterfaces/command_summary/parameter_intro/prog) to 512 characters.
- [CORE] Added a new transfer scheduler. [Details]()
- [CORE] Added balancing between server mode transfers and requester mode transfers.

[Back to top](#top)
