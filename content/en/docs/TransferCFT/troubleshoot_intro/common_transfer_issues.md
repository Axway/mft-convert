{
    "title": "Troubleshooting common transfer issues",
    "linkTitle": "Troubleshoot common transfer issues",
    "weight": "230"
}Diagi=302 
----------


| Event | Consequence |
| --- | --- |
| NET - Network link broken (cut-off, time-out) outside the connection phase. DIAGP is then set to VNRELI | D status - RETRY/COMMUT<br /> Up to &quot;RETRYM&quot; number or retries are performed for the transfer and the access data. If the number of retries reaches the value in the RETRYM parameter, Transfer CFT switches the access data.<br/> • The partner access data for the next retry will relate to the next IP address parameter (CFTNET command), or the next PROT parameter (CFTPART command). The restart counter is reset to 0.<br/> • If the protocol used is the last in the list, the transfer is either switched to the backup partner (IPART parameter of the CFTPART command) or aborted with code 405, while maintaining the diagnostic code (DIAGP) of the last retry |


The network link can break  during the connection phase or the transfer phase. If it occurs while trying to connect to the remote partner, remember that a remote partner is defined by 2 objects:

- CFTPART: Provides the SAP (Service Access Point) corresponding to the remote listening port (in TCP) (this name refers to the OSI naming for a network endpoint whatever the network protocol).

- CFTTCP: As TCP is the main network protocol used in Transfer CFT, it provides the HOST (numeric address or domain name).

 Most issues come from an unreachable address or port. To solve or avoid these issues:


| Check the issue and solutions | Description |
| --- | --- |
| The remote host is unreachable |   |
| Check that the remote host is reachable or that there is not a spelling mistake |  • If you have a domain name address, such as item-x123.axway.int, try using the ping command (ping &lt;host&gt;) with the corresponding numeric IP address.<br/><br/> • If the ping still fails, the remote host is unreachable.<br/> |
| The remote host is still unreachable |   |
| Check your hardware | Unplugged network cable...<br /> Remote host down due to fire, flood, etc. |
| Host is reachable but the remote port is not |   |
| Try with a telnet (&lt;host&gt; &lt;sap&gt;) or ftp (&lt;host&gt; &lt;sap&gt;) |  • The remote port is reachable via telnet when you have the following message: &quot;Escape character is '^]'.&quot; (Ctrl+$ to escape) <br/><br/> • The remote port is reachable via ftp when you have the following message: Connected to &lt;host&gt; (Ctrl + C to escape)<br/> |
| Host and port are reachable but I still have the diagnostic code 302 |   |
| Check the diagp error | If diagp = R 0 2f2, the remote partner closed the connection due to the following reasons:<br/> • Incompatibility between TCP and pTCP (the remote is configured in pTCP network protocol whereas you are in TCP)<br/><br/> • The cft.server.max_session value is not equal to 0 but is less than the MAXCNX one (in CFTNET object)<br/><br/> • The remote MAXCNX is reached<br/> |
|   | If diagp = R 01 280 or R 00 280, the timeout was reached because of:<br/> • A network issue (router, firewall, or other application...) <br/><br/> • An incompatibility in the IP protocol version:<br/><br/> • Check that locally and remotely all is configured with the same IP version (IPV4 , IPV6). &lt;/li&gt;&lt;li value=&quot;2&quot;&gt;Check the following uconf values: cft.ipv6.disable_listen cft.ipv6.disable_connect &lt;br/&gt;HOST value (CFTNET object).yes (the default value) &lt;/li&gt;&lt;/ol&gt; &lt;ul&gt;&lt;li&gt;&lt;p&gt;Try a Transfer CFT packet analyzer such as &lt;a class=&quot;Hyperlink_1&quot; href=&quot;https://www.wireshark.org/&quot;&gt;Wireshark&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;&lt;/ul&gt;&lt;/td&gt; |
| Still have the diagnostic code 302 and don't understand the diagp |   |
| Check the traces  | Check the contents of the following file:<br/> UNIX: Transfer_CFT/runtime/run folder &gt; cft.out file<br/> • Windows: Transfer_CFT\runtime\run folder &gt; cftnet.trc file<br/> |


Diagi=26x
---------


| Code | Event | Consequence |
| --- | --- | --- |
| 26x | SSL/TLS problem | When the network session is opened and if a secured connection is required, you may have an SSL handshake.<br /> The handshake:<br/> • is an exchange of messages between an SSL Client and an SSL Server<br/><br/> • authenticates the SSL Client and the SSL Server<br/><br/> • selects the cryptographic algorithms used to secure the communication<br /> <br /> In Transfer CFT, the SSL/TLS protocol is configured through:<br/><br/> • the CFTSSL object (CFT Configuration)<br/><br/> • some uconf values (such as cft.ssl.version_min and cft.ssl.version_max)<br/> |


 


| SSL objects | Description |
| --- | --- |
| SSL Client | Initiates the handshake (requester side). To select the SSL client profile:<br /> <br /> If SSL is defined in a CFTPART object (objects that define the remote partner and the SSL client profile which will be used, either directly by defining a SSL parameter)<br /> 1.CFTPART ID=PART1,PROT=PESITSSL,SSL=SSL1<br /> 2.CFTSSL ID=SSL1,DIRECT=CLIENT,...<br /> <br /> If SSL is not defined in a CFTPART object:<br /> 1.CFTPART ID=PART1,PROT=PESITSSL<br /> 2.CFTPROT ID=PESITSSL,SSL=SSL1,...<br /> 3.CFTSSL ID=SSL1,DIRECT=CLIENT,... |
| SSL Server | Answers to the client, authenticates itself and decides if the client must be authenticated too. As the handshake is executed before the PESIT session, the SSL profile selected is the one specified in the CFTPROT object. To select the SSL Server profile ( refers to the way the SSL profile is selected when CFT acts as a SSL server):<br /> 1.CFTPROT ID=PESITSSL,SSL=SSL2<br /> 2.CFTSSL ID=SSL2,DIRECT=SERVER,... |


Authentication implies having certificates and private keys. In Transfer CFT, these can be stored either in the local PKI database (CFT PKI) or in the PassPort PKI (PassPort PKI):

- Local PKI database

    In a Transfer CFT local PKI database, the private key is encrypted  by the PKIUTIL PKICER command (such as PKICER). the pkipassw parameter in the PKICER command is used to encrypt the private key in the local database  
    PKIUTIL PKICER ID=user1,iname=cert1.der,ikname=cert1k.der,pkipassw=&lt;password&gt; .  
    All private keys  must be stored in PKI database with the same &lt;password&gt;.  As Transfer CFT must have access to the private key, the CFTPARM object must refer to the same value in the PKIPASSW parameter 

<!-- -->

- PassPort PKI

    The private key is stored in an entity and can be accessed, using the PASSW parameter. This parameter is the password of for the CFTSSL object and allows Transfer CFT to access the private key.  
    For more information on other common SSL errors, see [Troubleshoot security errors](../admin_troubleshooting_server/troubleshoot_security).

Diagi=230 
----------

When the network session is opened and if a secured connection is required (SSL handshake), you may have a protocol session. During the protocol exchange:

- Connection

- Select File

- Open File

- Data Transfer

- End of Data transfer

- Close file

- Deselect File

- Disconnect

Information and data are exchanged during these phases. If an error occurs, a 230 diagnostic is generated and the diagnostic protocol provides more information on the cause of the protocol error.  
To find the cause of the error, it is highly recommended that you use the CFTATM traces with protocol trace on the server and on the client.


| Code | Event | Consequence |
| --- | --- | --- |
| 230 | PROT - Protocol error. A protocol error has been detected: DIAGP is set to the PeSIT or ODETTE code of the error detected | H status - ABORT, EXECE, ASIT EXIT |
| 730 | 1. PROT – Protocol error<br /> 2. PROT - (PeSIT) Transfer aborted by the remote end due to protocol error - PeSIT code: see DIAGP<br /> 3. PROT - (Odette) Reception of an ESID FPDU | H status - ABORT, EXECE, ASIT EXIT<br /> H status - ABORT, EXECE, ASIT EXIT<br /> <br /> H status - ABORT, EXECE |


The 230 diagnostic codes may have different diagnostic protocols formats, for example:


| Diagnostic codes | Event | Solution |
| --- | --- | --- |
| eEEsSS | Protocol error, where an unexpected event (EE) occurs and does not correspond with any SS state.<br /> As for the code name, e stands for event, s for state. EE (2 digits) for the event a type of event | If the problem reoccurs:<br/> • Set the CFTATM traces.<br/><br/> • Execute the cft_support collect, in order to have the maximum information. <br/> |
| PDU iNN | One element of the FPDU is not conform. The NN value indicates which parameter does not conform. | If the problem reproduces:<br/> • Set the CFTATM traces.<br/><br/> • Execute the cft_support collect, in order to have the maximum information.<br/> |

