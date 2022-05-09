{
    "title": "Troubleshoot security errors",
    "linkTitle": "Troubleshoot security errors ",
    "weight": "300"
}This section describes security related errors and troubleshooting tips.

Handshake errors
----------------

### Handshake error due to large certificate request

The TLS_ERR_FRAGMENT_CONSISTANCY message occurs during mutual authentication when the server sends a large number of CA in a certificate request. For example, the client sends a frame "Client_Hello" to initiate a new SSL session, but the Transfer CFT client cannot handle the reply from the server because it exceeds the RRUSIZE.

To troubleshoot:

1. Check the s/rrusize on the local Transfer CFT and set to the maximum value (32750).
1. Check on the remote server the number of trusted CA to be sent, and reduce if possible.

### No matching client certificate for SSL server

The customer receives a "certificate_request" type message, but it is empty. Transfer CFT cannot correctly respond to this message, and an error is generated.

******Client side******

In the catalog a 260 PKI 040 diagnostic displays, and in the log a message similar to the following is displayed:

```
CFTY13E CTX=200006 SSL Handshake local error [HANDSHAKE_FAILURE] CR=40 (Handshake failure)
CFTH11E Error Opening session <PART=HPX18SSL EV=VVTIMO ST=SUP01>
CFTT75E connect reject <IDTU=A00000BL PART=HPX18SSL IDF=SSL IDT=A2217045 260 PKI 040>
```

******Server side******

No diagnostic displays in catalog, but in the log a message similar to the following:

```
CFTY13E CTX=200006 SSL Handshake local error [HANDSHAKE_FAILURE] CR=40 (Handshake failure)
```

When using mutual authentication, the client does not have a certificate to provide to the remote SSL server . For example, the client has no user certificate corresponding to one of the CAs provided by the server.

> **Note**
>
> Tip  
> Analyze the situation on the partner side and correct depending on the server.

### No shared cipher

Handshake error on the server because there is no shared cipher. You should check the CFTSSL direct=server configuration and verify that the USERCID parameter is set to a valid certificate identifier.

```
CFTY13E CTX=210004 SSL Handshake local error [HANDSHAKE_FAILURE] CR=40 (Handshake failure: no shared cipher)
CFTY30E CTX=200003 SSL Handshake remote error [HANDSHAKE_FAILURE] CR=40 (Handshake failure: sslv3 alert handshake failure)
```

### Incorrect certificate format

In this log excerpt, there is an error "CR = 40" corresponding to a failure during the handshake phase.

This error may be related to the mode of insertion of client certificate, which is PEM format and not a certificate in DER.

```
CFTR12I RECV Treated for USER admin <IDTU=A00002EN PART=SEID IDF=D_615M>
Session parameters CFTT13I <IDTU=A00002EN PART=SEID IDF=D_615M IDT=J0615002 _ PROT=PROTSSL1 SAP=6335 HOST= 212.11.19.28>
CFTY19I PART = SEID = SSL Client SSLPROT1 opening session CTX = 20000c on task PID = 82928
CTX = 20000c CFTY21I Remote Server Certificate Accepted rootID = ROOTSEID
CTX = 20000c CFTY23I Client certificate ID = MUTESTCA rootID = ROOTSEID
CTX = 20000c CFTY13E SSL Handshake local error [HANDSHAKE_FAILURE] CR = 40
CFTH11E Error Opening session <PART=SEID EV=VVTIMO ST=SUP01>
CFTT75E connect reject <IDTU=A00002EN PART=SEID IDF=D_615M IDT=J0615002 260 TLSDOWN>
```

> **Note**
>
> Tip  
> A conversion of the certificate format is required.

#### No private key

This failure occurs during the handshake phase, and is related to using a user certificate that does not include the private key needed for encryption.

```
PART ARVAL CFTY19I = SSL = SSL_ARVAL customer opening session on task CTX = 200003 pid = 23630
CFTY21I CTX = 200003 Remote Server Certificate Accepted rootID = VERISIGNROOT
CFTY13E CTX = 200003 SSL Handshake local error [HANDSHAKE_FAILURE] CR = 40
CFTH11E Error Opening session <PART=ARVAL EV=VVTIMO ST=SUP01>
CFTT75E connect reject <IDTU=A000001R PART=ARVAL IDF=GEDTEST IDT=D2015423 260 TLSDOWN>
Requester CFTT56I file closed <IDTU=A000001R PART=ARVAL IDF=GEDTEST IDT=D2015423>
Requester CFTT54I file deselected <IDTU=A000001R PART=ARVAL IDF=GEDTEST IDT=D2015423>
```

> **Note**
>
> Tip  
> Insert the user certificate’s private key in the PKI based.

General errors
--------------

#### Encryption password issues

PKIPASSW is a password that is used to encrypt the private keys in the local PKI database (using the PKICER command with the PKIPASSW parameter) and also is used to decrypt the private key from the local database. Transfer CFT performs this operation using the PKIPASSW from the CFTPARM command.

These two parameters must be the same so that Transfer CFT can use the certificates in the database. If not, you may experience error messages similar to the following:

```
CFTY02Z>> CTX = 210004 SSLact () _ 7 SENDING DATA ALERT
CFTY02Z>> CTX = 15030100 210 004 020 233 >...... 3 <
CFTY02Z>> CTX = 210004 cftpki () = _ PHASE EndSession
```

#### Protocol is not correctly configured

In this scenario, Transfer CFT is trying to connect with a remote partner that is not configured for SSL. Check that the partner "SAP" in your configuration (PORT TCP) matches that of the remote SSL protocol.

In client mode if you have a combination of errors similar to the following, check that the remote partner is correctly configured for SSL.

```
CFTH11E PART1 PART = Error Opening session VNRELI EV = ST = SUP01
CFTT75E PART1 PART = IDF = TEST IDT connect reject = 260
CFTY11I CTX = 100003 PART1 PART = SSL = SSLPART1 Closing SSL client session
```

#### Insufficient security

In this scenario, two Transfer CFT’s have no cipher suite in common. The cipher suite to be used during the transfer is negotiated in the 'Client_hello' and 'Server_hello' frames. The server found no correspondence between the options presented and what is set on its side, so it returns an error.

******Client side******

```
CFTY13E CTX = 100003 SSL Handshake local error [INSUFFICIENT_SECURITY] CR = 71
```

******Server side******

```
CFTY13E CTX = 110004 SSL Handshake local error [close_notify] CR = 0
```

Check the 'ciphlist' setting in your definition CFTSSL.

### Unknown server certificate

In this scenario, the client does not recognize or accept the server's certificate (the server must be authenticated).

```
CTX = 2100e9 CFTY25I HOST = remote address 172.31.250.35
CTX = 2100e9 CFTY24I Server certificate ID = XMCA rootID = XMCA
CTX = 2100e9 CFTY13E SSL Handshake local error [UNKNOWN_CA] CR = 48
CTX = 2000e8 CFTY11I PART = SSL = GEM SSLG Closing SSL client session
PART CFTH61I GEM = IDS = 00232 PESIT Requester closed session
```

Check that the chain of authority from the server (ROOT certificate or intermediate) was inserted in the core PKI, and properly declared in the "ROOTCID" parameter of the "CFTSSL" profile used, where DIRECT=CLIENT.

### Protocol without SSL partner profile

In the log excerpt we see that to connect with the partner using the protocol PROT = SSLIPN there is no profile associated with SSL in its definition. Consequently, attempts to connect to an SSL listening partner fail.

```
CFTY20I PROT = SSL = SSLIPN IPSSLN opening session server task is 22057f CTX = PID = 21822
CTX = 22057f CFTY25I HOST = 172.31.24.119 remote address
CFTY12I CTX = 20057d = PROT = IPSSLN SSLIPN SSL server SSL session Closing
CFTY12I CTX = PROT = 21057th SSLIPN SSL = server SSL session Closing IPSSLN
CFTY12I CTX = PROT = 22057f SSLIPNSSL = IPSSLN server SSL session Closing
CFTN04E Synchronization error (release indication) SSLTID _ = 25886740 CS = CR =- 1 CtxRef NULL
```

TLS V1 and SSL V3 compatibility issues
--------------------------------------

It is recommended that you set the server to use TLSV1COMP. However, in Transfer CFT you cannot manually set the specific TLS version. If you set the CFTSSL object VERSION parameter to TLSV1 or TLSV1COMP, in client mode Transfer CFT introduces itself in TLS 1.2. The SSL communication is then performed using highest TLS version supported by the remote partner as defined by the TLS RFC.

When Transfer CFT is the client there are two possible scenarios that may lead to errors. When Transfer CFT is the server, it negotiates the TLS version and so no issue occurs.

If the client is configured for new compatibility mode and server is using the old, or the reverse, the log messages resemble the following.

```
CFTY13E CTX=200008 SSL Handshake local error [TLSPARSE] CR=1 (SSL_ERROR_ZERO_RETURN(6): NULL(err=0))
CFTH11E Error Opening session <PART=ST_QAPS EV=VVTIMO ST=CN0022>
CFTT75E connect reject <IDTU=A0000009 PART=ST_QAPS IDF=ST_QA_AP IDT=A2716390 260 TLSPARSE>
```

A message similar to the following is displayed in the catalog:

```
ST_QAPS SFK TK ST_QA_AP A2716390 0 0 260 TLSPARSE
```
<span id="Unknown"></span>

Unknown CA leads to a failed certificate verification
-----------------------------------------------------

In {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3 and lower, you can perform a SSL transfer even if the certificate chain is not complete (not signed by a ROOT CA). However, for {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.2.0 and higher, the certificate chain must be complete for a transfer to succeed.

****When working with multiple {{< TransferCFT/suitevariablesTransferCFTName  >}} versions****

Given the differences in certificate verification described above, you could encounter issues when performing SSL transfers between, for example {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3 and {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.4, depending on your configuration. This section presents an example of two Transfer CFTs where:

- CFT1: is server and is a {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3
- CFT2: is client and is a {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.4

****Example****

- CFT1: The server authenticates itself using the user certificate A. The certificate A is signed by the intermediate certificate B, which is signed by C. CFT1 imported the A certificate as the user and the B certificate as the root in its local database, meaning that the C certificate is not in the local database.
- CFT2: The client imported the B certificate as a root locally, the SSL config direct= client, rootcid=B.

****Results****

- During a simple authentication, CFT1 sends the A/B certificate chain, which CFT2 refuses because it is not complete (CFT2 requires the entire certificate chain).
- An error occurs: `SSL Handshake local error [HANDSHAKE_FAILURE] CR = 48 (Unknown CA: certificate verify failed)`

****Workaround****

To remedy this situation, on the client (CFT2 in our example):

1. Import the C certificate as the root in the local database.
1. Set the SSL configuration DIRECT=client, rootcid=(B, C).

****When migrating****

You should be mindful when migrating from {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3 or lower, that a previous configuration that was operational may not work with higher versions of {{< TransferCFT/suitevariablesTransferCFTName  >}} until you import and define the certificate chain as required for {{< TransferCFT/suitevariablesTransferCFTName  >}} versions 3.2.x and higher.

Recovery and trace analysis
---------------------------

This section describes how to collect trace information to help with troubleshooting.

### Activating traces

To enable an SSL trace, add the trace = 255 parameter to the control CFTSSL used in the exchange (default trace = 0 =&gt; no trace).

### Example activating a trace

```
CFTSSL ID = SSLCLIENT,
DIRECT = CLIENT,
ROOTCID = (ROOTCA, Intercable)
USERCID = User,
CIPHLIST = (47,10,9,1,2)
TRACE = 255
```

### Trace analysis

To analyze an SSL frame, check the log file and read the first 6 bytes of the SSL handshake data:

16 03 01 00 35 01

****Content type Version****

Major version l Minor version Fragment length Type message

Where:

- Content Type: determines the nature of the frame (0x: hexadecimal):
    -   0x14 change_cipher_specs
    -   0x15 alert
    -   0x16 handshake
    -   0x17 Application_Data
- Version: Determines the version of SSL used.
- Fragment length: specifies the length of the fragment.
- Message Type: Determines the type of message.

### Handshake frame alert

- 0x01 client_hello 0x01 warning
- 0x02 server_hello 0x02 fatal
- 0x0B certificate
- 0x0C server_key_exchange
- 0x0D certificate_request
- 0x0E server_hello_done
- 0x0F certificate_verify
- 0x10 client_key_exchange
- 0x14 finished

Authentication types
--------------------

This section presents two scenarios for establishing a session between client and server:

- Double authentication: the server requires the client to identify.
- Simple Authentication: server authentication only.

### Double Authentication

****Client side traces****

1. PART CFTY19I LOOPSSL1 = SSL = SSL_LOOP0 customer opening session on task CTX = 200003 pid = 3584  
    Information: Opening a client session with a context for isolating CTX transfer if there are many.
1. CFTY02Z&gt;&gt; CTX = 200003 ndata () _ 47 RECEIVED FROM HANDSHAKE DATA NETWORK
1. CFTY02Z&gt;&gt; CTX = 200003 ndata () _ DATA RECEIVED FROM HANDSHAKE 1458 NETWORK

- 0B =&gt; message Certificate
- The client receives the certificate sent by the server and verifies the authenticity of the certificate:
- FTY21I CTX = 200003 Remote Server Certificate Accepted rootID = ROOTCA
- =&gt; The client has accepted the server certificate

CFTY02Z&gt;&gt; CTX = 200003 ndata () _ 60 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 200003 16030100 33010100 370D0000 2F002D30 &gt;.... ...... 3 0 ... 7 &lt;

CFTY02Z&gt;&gt; CTX = 200003 09060355 04061302 6672310C 2B310B30&gt; .1.0 ... U. ... fr1. "

CFTY02Z&gt;&gt; CTX = 200003 300A0603 55040813 03696466 310E300C&gt; 0 ... U. ... idf1.0. "

CFTY02Z&gt;&gt; CTX = 200003 06035504 78776179 0A130541&gt; .. U. ... Axway &lt;

- 16030100 370D:
- = 0D&gt; receipt of the client's message Certificate_Request.

CFTY02Z&gt;&gt; CTX ndata = 200003 () 9 _ RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 040E0000 16030100 200 003 00 &gt;.........&lt;

- 16030100 040E:
- 0E =&gt; receiving the message Server_hello_done

CFTY23I CTX Client certificate ID = 200003 USER = = rootID ROOTCA

The client checks its internal datafile to find the pki certificate to be issued to the server.

CFTY02Z&gt;&gt; CTX = 200003 SSLact () _ SENDING DATA 1458 HANDSHAKE

CFTY02Z&gt;&gt; CTX = 200003 16030105 AD0B0005 A90005A6 0001DD30 &gt;............... 0 &lt;

CFTY02Z&gt;&gt; CTX = 200003 8201D930 03020102 820142A0 02010330&gt; ... B. .. 0 ....... 0 &lt;

CFTY02Z&gt;&gt; CTX = 200003 0D06092A 864886F7 0D010105 0500302B &gt;..... H. ....... 0. "

16030105 AD0B:

0B =&gt; The client sends a certificate message containing its certificate to identify itself to the server in response to the message he received Certificate_Request.

CTX = 200003 SSLact () _ 139 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200003 16030100 86100000 82008031 7720BA19 &gt;........... 1w ... &lt;

CFTY02Z&gt;&gt; CTX = 200003 B64A8529 33DFEB17 776E82D7 B88AFBA2&gt;. ... .3 Wn ......&lt; J.

16030100 8610:

10 =&gt; client_key_exchange message containing the pre-master key.

CFTY02Z&gt;&gt; CTX = 200003 SSLact () _ 139 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200003 16030100 82008067 860F0000 F81CB9ED &gt;........... g. ... &lt;

CFTY02Z&gt;&gt; CTX = 200003 05B70CBB E22544FC 9A455CA9 DCC9FDC9 &gt;...... D.. .....&lt; E.

CFTY02Z&gt;&gt; CTX = 200003 1947240B DA78B4BD EFCE3C41 4C79325F&gt;. ... X. .. G. ALy2. "

16030100 860F:

0F =&gt; Certificate_Verify message sent to the server informing it that the certificate was accepted.

CFTY02Z&gt;&gt; CTX = 200003 SSLact () _ 6 SENDING DATA HANDSHAKE

CFTY02Z&gt;&gt; CTX = 14030100 200 003 0101 &gt;......&lt;

- The header is: 14030100 0101
- 14: change_cipher_specs
- 0301: Version SSL 3.1
- 00 01: Length of the message (always 1 byte).
- 01: it was only one type of message.
- The client informs the server that it will switch to encrypted.

CFTY02Z&gt;&gt; CTX = 200003 SSLact () _ 53 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200003 16030100 30CADB9A 0A63B212 AE215854 0 &gt;.... .... c. ...

XT &lt;

CFTY02Z&gt;&gt; CTX = 200003 04851424 63864DF8 8A332ADA 730F266A &gt;..... s. .. 3. Jc.M. &lt;

CFTY02Z&gt;&gt; CTX = 200003 1518BD3E EAB402F9 433F1873 4A678C13 &gt;........ C.. SJG .. &lt;

CFTY02Z&gt;&gt; CTX = 200003 1BE73E6D CE&gt; ... m. &lt;

16030100 30CA: The client sends an encrypted Finished message.

CFTY02Z&gt;&gt; CTX = 200003 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK

CFTY02Z&gt;&gt; CTX = 14030100 200 003 0101 &gt;......&lt;

14030100 0101: Change_cipher_specs receiving the message sent by the server.

CTX = 200003 16030100 30924A9E E1BFCB45 A013E3F5 &gt;.... 0.J. ... E. ... "

CFTY02Z&gt;&gt; CTX = 200003 DAE6ED53 3DA5C690 81ADC93C 3A4B079A&gt; ... ........ S. K.. "

CFTY02Z&gt;&gt; CTX = 200003 80839006 59710836 BAA16B88 7CB5087B &gt;...... k.Yq.6 ....&lt;

CFTY02Z&gt;&gt; CTX = 200003 F0601CA1 34 &gt;.... 4 &lt;

16030100 3092: Finished receiving the message sent by the server.

CFTY02Z&gt;&gt; CTX = 200003 DONE SUCCESSFULLY HANDSHAKE

CTX = 200003 = PART = SSL_LOOP0 LOOPSSL1 SSL Client session ESTABLISHED CIPHER AUTH = 47 = BOTH

The operation completed successfully handshake. The two entities, client and server, can start sending data on the session they just established.

CFTY02Z&gt;&gt; CTX ndata = 200003 () 37 _ APPLICATION DATA RECEIVED FROM NETWORK

CFTY02Z&gt;&gt; CTX = 200003 17030100 481E9681 2042C8C3 B8203F62 &gt;..... B.. ..... H. b &lt;

The client begins to receive data from the server.

### Server side trace

1. CFTY20I PESITSSL PROT = SSL = server SSLPESIT opening session on task CTX = 210004 pid = 3584
1. CFTY24I CTX = 210004 Server certificate ID = USER = rootID ROOTCA
1. CFTY02Z&gt;&gt; CTX = 210004 SSLact () _ 60 HANDSHAKE SENDING DATA
1. CFTY02Z&gt;&gt; CTX = 210004 SSLact () _ 9 HANDSHAKE SENDING DATA
1. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ DATA RECEIVED FROM HANDSHAKE 1458 NETWORK
1. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ 139 RECEIVED FROM HANDSHAKE DATA NETWORK
1. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ 139 RECEIVED FROM HANDSHAKE DATA NETWORK
1. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ 53 RECEIVED FROM HANDSHAKE DATA NETWORK
1. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK
10. CFTY02Z&gt;&gt; CTX = 210004 SSLact () _ 53 HANDSHAKE SENDING DATA
11. CFTY02Z&gt;&gt; CTX = 210004 DONE SUCCESSFULLY HANDSHAKE

CTX = 210004 = PROT = SSLPESIT PESITSSL SSL server session ESTABLISHED CIPHER AUTH = 47 = BOTH

Operation "handshake" was successfully completed with an SSL connection established for a server by double authentication and encryption selected: 47.

1. CFTY02Z&gt;&gt; CTX ndata = 210004 () 53 _ APPLICATION DATA RECEIVED FROM NETWORK

CFTY02Z&gt;&gt; CTX = 210004 17030100 301EE379 BCF9F73E 544D6F6E &gt;.... 0 .. y. ... TMON &lt;

CFTY02Z&gt;&gt; CTX = 210004 DD253D7F 1C1368E6 C4323E6F 10C00E2D &gt;...... h. .2. ... O. &lt;

CFTY02Z&gt;&gt; CTX = 210004 65530975 3749496E 85DE9C7E 3E930EE5 &gt;.... eS.u7IIn ....&lt;

CFTY02Z&gt;&gt; CTX = 210004 0E C80B6B48&gt; .. kH. "

The server starts sending data (17: Application Data).

Simple authentication
---------------------

### Client side traces

1. PART CFTY19I LOOPSSL1 = SSL = SSL_LOOP0 customer opening session on task CTX = 200005 pid = 5056
1. CTX ndata = 200005 () 47 _ DATA RECEIVED FROM HANDSHAKE NETWORK
1. CTX = 200005 ndata () _ DATA RECEIVED FROM HANDSHAKE 1458 NETWORK
1. CTX ndata = 200005 () 9 _ DATA RECEIVED FROM HANDSHAKE NETWORK
1. CFTY02Z&gt;&gt; CTX = 200005 SSLact () _ 139 HANDSHAKE SENDING DATA
1. CFTY02Z&gt;&gt; CTX = 200005 SSLact () _ 6 SENDING DATA HANDSHAKE
1. CFTY02Z&gt;&gt; CTX = 200005 SSLact () _ 53 HANDSHAKE SENDING DATA
1. CFTY02Z&gt;&gt; CTX = 200005 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK
1. CTX ndata = 200005 () 53 _ DATA RECEIVED FROM HANDSHAKE NETWORK

CFTY02Z&gt;&gt; CTX = 200005 16030100 30139F01 1543DF7A AFB9F25B &gt;.... 0 .... ... Cz &lt;

CFTY02Z&gt;&gt; CTX = 200005 5A133F29 6CE01311 FCB18C52 12029A63&gt; Z. .. l. ..... R. .. c &lt;

16030100 3013: reception of the message finished.

CTX = 200005 DONE SUCCESSFULLY HANDSHAKE

Client session ESTABLISHED = 47 CIPHER AUTH = SERVER

So the client can begin to transmit and receive data over the connection he has to establish:

CTX ndata = 200005 () 37 _ APPLICATION DATA RECEIVED FROM NETWORK

CTX = 200005 17030100 40E55126 20E7554B ED8AD7E2 &gt;...... Q. ...... UK &lt;

CTX = 200005 PDataRq () _ 165 SENDING APPLICATION DATA

CTX = 200005 17030100 A0B63A45 A3CE4959 08B00BB7 E. &gt;........ ....&lt; IY

### Server side

****CFTY20I PESITSSL PROT = SSL = server SSLPESIT opening session on task CTX = 210006 pid = 5056****

1. CFTY02Z&gt;&gt; CTX = 210006 ndata () _ 58 RECEIVED FROM HANDSHAKE DATA NETWORK
1. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ 47 HANDSHAKE SENDING DATA
1. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ SENDING DATA 1458 HANDSHAKE
1. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ 9 HANDSHAKE SENDING DATA
1. CTX = 210006 ndata () _ 139 RECEIVED FROM HANDSHAKE DATA NETWORK
1. CFTY02Z&gt;&gt; CTX = 210006 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK
1. CFTY02Z&gt;&gt; CTX = 210006 ndata () _ 53 RECEIVED FROM HANDSHAKE DATA NETWORK
1. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ 6 SENDING DATA HANDSHAKE
1. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ 53 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 210006 16030100 30139F01 1543DF7A AFB9F25B &gt;.... 0 .... ... Cz &lt;

CFTY02Z&gt;&gt; CTX = 210006 5A133F29 6CE01311 FCB18C52 12029A63&gt; Z. .. l. ..... R. .. c &lt;

CFTY02Z&gt;&gt; CTX = 210006 02F0638B 26FFCD2C 8679D689 071883A6&gt; .. c.. .........&lt; Y.

CFTY02Z&gt;&gt; CTX = 210006 92B8493F FB&gt; .. I.. "

16030100 3013: Finished sending the message.

CTX = 210006 DONE SUCCESSFULLY HANDSHAKE

PROT = = SSLPESIT PESITSSL SSL server session ESTABLISHED = 47 CIPHER AUTH = SERVER

Phase handshake completed successfully after a server authentication, using sequence 47 for encryption.

CFTY02Z&gt;&gt; CTX ndata = 210006 () 53 _ APPLICATION DATA RECEIVED FROM NETWORK

CFTY02Z&gt;&gt; CTX = 210006 17030100 8157735C 3055D0D3 6FB8A866 &gt;.... 0U Ws.o. .... F &lt;

CFTY02Z&gt;&gt; CTX = 210006 33DAA956 2A00C60E 2B97F46A DBEDA18C&gt; 3 .. V. ...... j. ... &lt;

CFTY02Z&gt;&gt; CTX = 210006 FB9F36D0 DAE30B77 1CAFBEA3 A0D58639&gt; .... w. .. 6 ...... 9 &lt;

CFTY02Z&gt;&gt; CTX = 210006 B9D3A058 C4&gt; ... X &lt;

CFTY02Z&gt;&gt; CTX = 210006 PDataRq () _ 37 SENDING APPLICATION DATA

CFTY02Z&gt;&gt; CTX = 210006 17030100 40E55126 20E7554B ED8AD7E2 &gt;...... Q. ...... UK &lt;

CFTY02Z&gt;&gt; CTX = 210006 C5D3AE21 345CE65A BCB403B3 D65F9DEC &gt;.... 4 .......&lt; Z. ..

CFTY02Z&gt;&gt; CTX = 210006 BF6CDD76 5D. "L.v. &lt;
