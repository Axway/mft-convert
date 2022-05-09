{
    "title": "Recovery and trace analysis",
    "linkTitle": "Troubleshoot SSL and TLS ",
    "weight": "330"
}This section describes how to collect trace information to help with troubleshooting.

Activating traces
-----------------

To enable an SSL trace, add the parameter `trace = 255` to the control CFTSSL used in the exchange (default trace = 0 =&gt; no trace).

### Example activating a trace

`CFTSSL  ID          = SSLCLIENT,`

`DIRECT      = CLIENT,`

`ROOTCID = (ROOTCA, Intercable, ROOT_Du_Partenaire, Inter_Partenaire)`

`USERCID     = User,`

`CIPHLIST    = (47,10,9,1,2)`

`TRACE       = 255`

### Trace analysis

To analyze an SSL frame, read the first 6 bytes:

`1   8                           24                40        48`

`Content type    Version`

`Major version     l   Minor version Fragment length Type message`

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

`0x01   client_hello 0x01   warning`

`0x02   server_hello 0x02   fatal`

`0x0B  certificate`

`0x0C  server_key_exchange`

`0x0D  certificate_request`

`0x0E   server_hello_done`

`0x0F   certificate_verify`

`0x10   client_key_exchange`

`0x14   finished`

Authentication types
--------------------

This section presents two scenarios for establishing a session between client and server:

- Double authentication: the server requires the client to identify.
- Simple Authentication: server authentication only.

### Double Authentication

#### Client side traces

1. PART CFTY19I LOOPSSL1 = SSL = SSL_LOOP0 customer opening session on task CTX = 200003 pid = 3584

Information: opening a client session with a context for isolating CTX transfer in case there would be many.

CFTY02Z&gt;&gt; CTX = 200003 16030100 35010000 3103014D AECAB8AD &gt;.... 5 ... 1 .. M. ... "

CFTY02Z&gt;&gt; CTX = 200003 355E939D EFFE380E 7B5C37C9 18FC993A&gt; 5 ... 7 ..... 8 .....&lt;

CFTY02Z&gt;&gt; CTX = 200003 66A90F82 E271C992 D0972100 000A002F&gt; q. .........&lt; f. ...

Where the header is: 01 16 0301 0035

= 16&gt; Handshake

0301 =&gt; SSL V 3.1

0035 =&gt; length of the message below. (53 bytes)

= 01&gt; message "Client hello" first message sent by the client to establish an SSL connection.

2. CFTY02Z&gt;&gt; CTX = 200003 ndata () _ 47 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 200003 16030100 2A020000 2603014D AECAB899 &gt;........... M. ... "

CFTY02Z&gt;&gt; CTX = 200003 391A07A0 9B9E37A8 22A2704C CCD0179E&gt; 9 ..... 7 ... ....&lt; pL

CFTY02Z&gt;&gt; CTX = 200003 69124259 8D79970A AC49B900 002F00&gt; i.BY.y. I. .. ....&lt;

16030100 2A02:

02 =&gt; The client receives the message Server_hello.

3. CFTY02Z&gt;&gt; CTX = 200003 ndata () _ DATA RECEIVED FROM HANDSHAKE 1458 NETWORK

CFTY02Z&gt;&gt; CTX = 200003 16030105 AD0B0005 A90005A6 0001DD30 &gt;............... 0 &lt;

CFTY02Z&gt;&gt; CTX = 200003 8201D930 03020102 820142A0 02010330&gt; ... B. .. 0 ....... 0 &lt;

CFTY02Z&gt;&gt; CTX = 200003 0D06092A 864886F7 0D010105 0500302B &gt;..... H. ....... 0. "

16030105 AD0B:

0B =&gt; message Certificate

The client receives the certificate sent by the server and shall verify the authenticity of the certificate:

FTY21I CTX = 200003 Remote Server Certificate Accepted rootID = ROOTCA

=&gt; The client has accepted the server certificate

4. CFTY02Z&gt;&gt; CTX = 200003 ndata () _ 60 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 200003 16030100 33010100 370D0000 2F002D30 &gt;.... ...... 3 0 ... 7 &lt;

CFTY02Z&gt;&gt; CTX = 200003 09060355 04061302 6672310C 2B310B30&gt; .1.0 ... U. ... fr1. "

CFTY02Z&gt;&gt; CTX = 200003 300A0603 55040813 03696466 310E300C&gt; 0 ... U. ... idf1.0. "

CFTY02Z&gt;&gt; CTX = 200003 06035504 78776179 0A130541&gt; .. U. ... Axway &lt;

16030100 370D:

= 0D&gt; receipt of the client's message Certificate_Request.

5. CFTY02Z&gt;&gt; CTX ndata = 200003 () 9 _ RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 040E0000 16030100 200 003 00 &gt;.........&lt;

16030100 040E:

0E =&gt; receiving the message Server_hello_done

6. CFTY23I CTX Client certificate ID = 200003 USER = = rootID ROOTCA

The client checks its internal datafile to find the pki certificate to be issued to the server.

CFTY02Z&gt;&gt; CTX = 200003 SSLact () _ SENDING DATA 1458 HANDSHAKE

CFTY02Z&gt;&gt; CTX = 200003 16030105 AD0B0005 A90005A6 0001DD30 &gt;............... 0 &lt;

CFTY02Z&gt;&gt; CTX = 200003 8201D930 03020102 820142A0 02010330&gt; ... B. .. 0 ....... 0 &lt;

CFTY02Z&gt;&gt; CTX = 200003 0D06092A 864886F7 0D010105 0500302B &gt;..... H. ....... 0. "

16030105 AD0B:

0B =&gt; The client sends a certificate message containing its certificate to identify itself to the server in response to the message he received Certificate_Request.

7. CTX = 200003 SSLact () _ 139 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200003 16030100 86100000 82008031 7720BA19 &gt;........... 1w ... &lt;

CFTY02Z&gt;&gt; CTX = 200003 B64A8529 33DFEB17 776E82D7 B88AFBA2&gt;. ... .3 Wn ......&lt; J.

16030100 8610:

10 =&gt; client_key_exchange message containing the pre-master key.

8. CFTY02Z&gt;&gt; CTX = 200003 SSLact () _ 139 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200003 16030100 82008067 860F0000 F81CB9ED &gt;........... g. ... &lt;

CFTY02Z&gt;&gt; CTX = 200003 05B70CBB E22544FC 9A455CA9 DCC9FDC9 &gt;...... D.. .....&lt; E.

CFTY02Z&gt;&gt; CTX = 200003 1947240B DA78B4BD EFCE3C41 4C79325F&gt;. ... X. .. G. ALy2. "

16030100 860F:

0F =&gt; Certificate_Verify message sent to the server informing it that the certificate was accepted.

9. CFTY02Z&gt;&gt; CTX = 200003 SSLact () _ 6 SENDING DATA HANDSHAKE

CFTY02Z&gt;&gt; CTX = 14030100 200 003 0101 &gt;......&lt;

The header is: 14030100 0101

14: change_cipher_specs

0301: Version SSL 3.1

00 01: Length of the message (always 1 byte).

01: it was only one type of message.

The client informs the server that it will switch to encrypted.

10. CFTY02Z&gt;&gt; CTX = 200003 SSLact () _ 53 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200003 16030100 30CADB9A 0A63B212 AE215854 0 &gt;.... .... c. ...

XT &lt;

CFTY02Z&gt;&gt; CTX = 200003 04851424 63864DF8 8A332ADA 730F266A &gt;..... s. .. 3. Jc.M. &lt;

CFTY02Z&gt;&gt; CTX = 200003 1518BD3E EAB402F9 433F1873 4A678C13 &gt;........ C.. SJG .. &lt;

CFTY02Z&gt;&gt; CTX = 200003 1BE73E6D CE&gt; ... m. &lt;

16030100 30CA: The client sends an encrypted Finished message.

11. CFTY02Z&gt;&gt; CTX = 200003 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK

CFTY02Z&gt;&gt; CTX = 14030100 200 003 0101 &gt;......&lt;

14030100 0101: Change_cipher_specs receiving the message sent by the server.

12. CTX = 200003 16030100 30924A9E E1BFCB45 A013E3F5 &gt;.... 0.J. ... E. ... "

CFTY02Z&gt;&gt; CTX = 200003 DAE6ED53 3DA5C690 81ADC93C 3A4B079A&gt; ... ........ S. K.. "

CFTY02Z&gt;&gt; CTX = 200003 80839006 59710836 BAA16B88 7CB5087B &gt;...... k.Yq.6 ....&lt;

CFTY02Z&gt;&gt; CTX = 200003 F0601CA1 34 &gt;.... 4 &lt;

16030100 3092: Finished receiving the message sent by the server.

13. CFTY02Z&gt;&gt; CTX = 200003 DONE SUCCESSFULLY HANDSHAKE

CTX = 200003 = PART = SSL_LOOP0 LOOPSSL1 SSL Client session ESTABLISHED CIPHER AUTH = 47 = BOTH

The operation completed successfully handshake. The two entities, client and server, can start sending data on the session they just established.

14. CFTY02Z&gt;&gt; CTX ndata = 200003 () 37 _ APPLICATION DATA RECEIVED FROM NETWORK

CFTY02Z&gt;&gt; CTX = 200003 17030100 481E9681 2042C8C3 B8203F62 &gt;..... B.. ..... H. b &lt;

The client begins to receive data from the server.

#### Server side trace

1. CFTY20I PESITSSL PROT = SSL = server SSLPESIT opening session on task CTX = 210004 pid = 3584

Information: log server, with a context for isolating CTX transfer in case there would be many.

CFTY02Z&gt;&gt; CTX = 210004 ndata () _ 58 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 210004 16030100 35010000 3103014D AECAB8AD &gt;.... 5 ... 1 .. M. ... "

CFTY02Z&gt;&gt; CTX = 210004 355E939D EFFE380E 7B5C37C9 18FC993A&gt; 5 ... 7 ..... 8 .....&lt;

CFTY02Z&gt;&gt; CTX = 210004 66A90F82 E271C992 D0972100 000A002F&gt; q. .........&lt; f. ...

CFTY02Z&gt;&gt; CTX = 210004 000A0009 00010002 0100 &gt;..........&lt;

The header is: 01 16 0301 0035

= 16&gt; Handshake

0301 =&gt; SSL V 3.1

0035 =&gt; length of the message below.

01 =&gt; Client message _hello

Receiving a message to establish an SSL connection.

2. CFTY02Z&gt;&gt; CTX = 210004 SSLact () _ 47 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 210004 16030100 2A020000 2603014D AECAB899 &gt;........... M. ... "

CFTY02Z&gt;&gt; CTX = 210004 391A07A0 9B9E37A8 22A2704C CCD0179E&gt; 9 ..... 7 ... ....&lt; pL

CFTY02Z&gt;&gt; CTX = 210004 69124259 8D79970A AC49B900 002F00&gt; i.BY.y. I. .. ....&lt;

The header is: 02 16 0301 0035

= 16&gt; Handshake

0301 =&gt; SSL V 3.1

0035 =&gt; length of the message below.

= 02&gt; message Server_hello

First message sent to the client as a response to the message received Client_hello.

3. CFTY24I CTX = 210004 Server certificate ID = USER = rootID ROOTCA

The server consults its internal datafile to find the issued PKI certificate.

CTX = 210004 SSLact () _ SENDING DATA 1458 HANDSHAKE

CFTY02Z&gt;&gt; CTX = 210004 16030105 AD0B0005 A90005A6 0001DD30 &gt;............... 0 &lt;

CFTY02Z&gt;&gt; CTX = 210004 8201D930 03020102 820142A0 02010330&gt; ... B. .. 0 ....... 0 &lt;

CFTY02Z&gt;&gt; CTX = 210004 0D06092A 864886F7 0D010105 0500302B &gt;..... H. ....... 0. "

The header is: 16 0301 05AD 0B

= 16&gt; Handshake

0301 =&gt; SSL V 3.1

05AD =&gt; length of the message below.

0B =&gt; Certificate message

The server sends its certificate to the client.

4. CFTY02Z&gt;&gt; CTX = 210004 SSLact () _ 60 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 210004 16030100 33010100 370D0000 2F002D30 &gt;.... ...... 3 0 ... 7 &lt;

CFTY02Z&gt;&gt; CTX = 210004 09060355 04061302 6672310C 2B310B30&gt; .1.0 ... U. ... fr1. "

CFTY02Z&gt;&gt; CTX = 210004 300A0603 55040813 03696466 310E300C&gt; 0 ... U. ... idf1.0. "

CFTY02Z&gt;&gt; CTX = 210004 06035504 78776179 0A130541&gt; .. U. ... Axway &lt;

16030100 370D:

= 0D&gt; message Certificate_request

The server requests the client certificate.

5. CFTY02Z&gt;&gt; CTX = 210004 SSLact () _ 9 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 040E0000 16030100 210 004 00 &gt;.........&lt;

16030100 040E:

0E: server_hello_done message to inform the client that it has finished sending messages.

6. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ DATA RECEIVED FROM HANDSHAKE 1458 NETWORK

CFTY02Z&gt;&gt; CTX = 210004 16030105 AD0B0005 A90005A6 0001DD30 &gt;............... 0 &lt;

CFTY02Z&gt;&gt; CTX = 210004 8201D930 03020102 820142A0 02010330&gt; ... B. .. 0 ....... 0 &lt;

16030105 AD0B:

0B =&gt; The server receives the client certificate and performs the audit:

CFTY22I CTX = 210004 Remote client certificate Accepted rootID = ROOTCA

The server accepted the client certificate.

7. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ 139 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 210004 16030100 86100000 82008031 7720BA19 &gt;........... 1w ... &lt;

CFTY02Z&gt;&gt; CTX = 210004 B64A8529 33DFEB17 776E82D7 B88AFBA2&gt;. ... .3 Wn ......&lt; J.

16030100 8610:

10 =&gt; Client_Key_Exchange receiving the message (receipt of the pre-master key).

8. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ 139 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 210004 16030100 82008067 860F0000 F81CB9ED &gt;........... g. ... &lt;

CFTY02Z&gt;&gt; CTX = 210004 05B70CBB E22544FC 9A455CA9 DCC9FDC9 &gt;...... D.. .....&lt; E.

16030100 860F:

0F =&gt; Certificate_Verify message to inform the client that the certificate was accepted.

9. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ 53 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 210004 16030100 30CADB9A 0A63B212 AE215854 0 &gt;.... .... c. ... XT &lt;

CFTY02Z&gt;&gt; CTX = 210004 04851424 63864DF8 8A332ADA 730F266A &gt;..... s. .. 3. Jc.M. &lt;

CFTY02Z&gt;&gt; CTX = 210004 1518BD3E EAB402F9 433F1873 4A678C13 &gt;........ C.. SJG .. &lt;

CFTY02Z&gt;&gt; CTX = 210004 1BE73E6D CE&gt; ... m. &lt;

16030100 30CA: Finished message is received.

10. CFTY02Z&gt;&gt; CTX = 210004 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK

CFTY02Z&gt;&gt; CTX = 14030100 210 004 0101 &gt;......&lt;

140301000101: The server informs the client that he will switch to encrypted. (Message Change_cipher_Specs)

11. CFTY02Z&gt;&gt; CTX = 210004 SSLact () _ 53 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 210004 16030100 30924A9E E1BFCB45 A013E3F5 &gt;.... 0.J. ... E. ... "

CFTY02Z&gt;&gt; CTX = 210004 DAE6ED53 3DA5C690 81ADC93C 3A4B079A&gt; ... ........ S. K.. "

CFTY02Z&gt;&gt; CTX = 210004 80839006 59710836 BAA16B88 7CB5087B &gt;...... k.Yq.6 ....&lt;

CFTY02Z&gt;&gt; CTX = 210004 F0601CA1 34 &gt;.... 4 &lt;

16030100 30CA: Sends the message Finished

CFTY02Z&gt;&gt; CTX = 210004 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK

CFTY02Z&gt;&gt; CTX = 14030100 210 004 0101 &gt;......&lt;

14030100 0101: Change_Cipher_Specs receiving the message.

12. CFTY02Z&gt;&gt; CTX = 210004 DONE SUCCESSFULLY HANDSHAKE

CTX = 210004 = PROT = SSLPESIT PESITSSL SSL server session ESTABLISHED CIPHER AUTH = 47 = BOTH

Operation "handshake" was successfully completed with an SSL connection established for a server by double authentication and encryption selected: 47.

13. CFTY02Z&gt;&gt; CTX ndata = 210004 () 53 _ APPLICATION DATA RECEIVED FROM NETWORK

CFTY02Z&gt;&gt; CTX = 210004 17030100 301EE379 BCF9F73E 544D6F6E &gt;.... 0 .. y. ... TMON &lt;

CFTY02Z&gt;&gt; CTX = 210004 DD253D7F 1C1368E6 C4323E6F 10C00E2D &gt;...... h. .2. ... O. &lt;

CFTY02Z&gt;&gt; CTX = 210004 65530975 3749496E 85DE9C7E 3E930EE5 &gt;.... eS.u7IIn ....&lt;

CFTY02Z&gt;&gt; CTX = 210004 0E C80B6B48&gt; .. kH. "

The server starts sending data (17: Application Data).

### Simple authentication

#### Client side traces

1. PART CFTY19I LOOPSSL1 = SSL = SSL_LOOP0 customer opening session on task CTX = 200005 pid = 5056

Opening a new SSL session

CFTY02Z&gt;&gt; CTX = 200005 SSLact () _ 58 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200005 16030100 35010000 3103014D AFF79D6B &gt;.... 5 ... 1 .. M. .. k &lt;

CFTY02Z&gt;&gt; CTX = 200005 190984E6 3941618C BEF4666E 85DFF23F&gt; .. .... fn .....&lt; 9AA

CFTY02Z&gt;&gt; CTX = 200005 6114ED70 F483C3F0 5149F500 000A002F&gt; a.. ......&lt; IQ p. ...

CFTY02Z&gt;&gt; CTX = 200005 000A0009 00010002 0100 &gt;..........&lt;

16030100 3501: Sending message Client_hello

2. CTX ndata = 200005 () 47 _ DATA RECEIVED FROM HANDSHAKE NETWORK

CFTY02Z&gt;&gt; CTX = 200005 16030100 2A020000 2603014D AFF79DA5 &gt;........... M. ... "

CFTY02Z&gt;&gt; CTX = 200005 73149449 B00E24C8 DB56710A 8ACE246E&gt; s.. .... I. .... Vq n &lt;

CFTY02Z&gt;&gt; CTX = 200005 59299D5C 690F32BE 8E2B2900 002F00&gt; i.2.Y. .........&lt;

16030100 2A02: Server_hello reception.

3. CTX = 200005 ndata () _ DATA RECEIVED FROM HANDSHAKE 1458 NETWORK

CFTY02Z&gt;&gt; CTX = 200005 16030105 AD0B0005 A90005A6 0001DD30 &gt;............... 0 &lt;

CFTY02Z&gt;&gt; CTX = 200005 8201D930 03020102 820142A0 02010330&gt; ... B. .. 0 ....... 0 &lt;

16030105 AD0B: receiving the (server) Certificate message.

CFTY21I CTX = 200005 Remote Server Certificate Accepted rootID = ROOTCA

After verification, the customer accepts the certificate.

4. CTX ndata = 200005 () 9 _ DATA RECEIVED FROM HANDSHAKE NETWORK

CFTY02Z&gt;&gt; CTX = 040E0000 16030100 200 005 00 &gt;.........&lt;

16030100 040E: Server_hello_done receiving the message.

5. CFTY02Z&gt;&gt; CTX = 200005 SSLact () _ 139 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200005 16030100 86100000 82008066 3C1D98DA &gt;........... f. ... "

CFTY02Z&gt;&gt; CTX = 200005 F9DE2268 3472A9A6 767FC251 188B14B1&gt; H4R ... .. v.. ... Q. "

16030100 8610: Sending message Clien_Key_Exchange (containing the key pre-master)

6. CFTY02Z&gt;&gt; CTX = 200005 SSLact () _ 6 SENDING DATA HANDSHAKE

CFTY02Z&gt;&gt; CTX = 14030100 200 005 0101 &gt;......&lt;

14030100 0101: Sending message Change_Cipher_Specs.

7. CFTY02Z&gt;&gt; CTX = 200005 SSLact () _ 53 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 200005 16030100 0C 304319F2 E3B4FC0E 1E4E8236 &gt;.... N.6 ....... &lt;

CFTY02Z&gt;&gt; CTX = 200005 DD323B1E 745E023A 31EEBDAB 2E271880&gt; .2 t. .. 1 .. .......&lt;

CFTY02Z&gt;&gt; CTX = 200005 AA2D9520 B4F43E0E D7ED0598 38275B5D &gt;............ 8 ... "

CFTY02Z&gt;&gt; CTX = 200005 74463CDE D8&gt; tF ... &lt;

16030100 3043: Finished sending the message.

8. CFTY02Z&gt;&gt; CTX = 200005 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK

CFTY02Z&gt;&gt; CTX = 14030100 200 005 0101 &gt;......&lt;

14030100 0101: Change_Cipher_Specs receiving the message.

9. CTX ndata = 200005 () 53 _ DATA RECEIVED FROM HANDSHAKE NETWORK

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

#### Server side

CFTY20I PESITSSL PROT = SSL = server SSLPESIT opening session on task CTX = 210006 pid = 5056

1. CFTY02Z&gt;&gt; CTX = 210006 ndata () _ 58 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 210006 16030100 35010000 3103014D AFF79D6B &gt;.... 5 ... 1 .. M. .. k &lt;

CFTY02Z&gt;&gt; CTX = 210006 190984E6 3941618C BEF4666E 85DFF23F&gt; .. .... fn .....&lt; 9AA

16030100 3501: receiving a message Client_hello.

2. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ 47 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 210006 16030100 2A020000 2603014D AFF79DA5 &gt;........... M. ... "

CFTY02Z&gt;&gt; CTX = 210006 73149449 B00E24C8 DB56710A 8ACE246E&gt; s.. .... I. .... Vq n &lt;

16030100 2A02: Sending a message in response to Server_hello client_hello.

3. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ SENDING DATA 1458 HANDSHAKE

CFTY02Z&gt;&gt; CTX = 210006 16030105 AD0B0005 A90005A6 0001DD30 &gt;............... 0 &lt;

CFTY02Z&gt;&gt; CTX = 210006 8201D930 03020102 820142A0 02010330&gt; ... B. .. 0 ....... 0 &lt;

CFTY02Z&gt;&gt; CTX = 210006 0D06092A 864886F7 0D010105 0500302B &gt;..... H. ....... 0. "

AD0B 16030105: Sending the message Certificate.

4. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ 9 HANDSHAKE SENDING DATA

CFTY02Z&gt;&gt; CTX = 040E0000 16030100 210 006 00 &gt;.........&lt;

16030100 040E: Send Server_hello_done.

5. CTX = 210006 ndata () _ 139 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 210006 16030100 86100000 82008066 3C1D98DA &gt;........... f. ... "

CFTY02Z&gt;&gt; CTX = 210006 F9DE2268 3472A9A6 767FC251 188B14B1&gt; ... H4R v. ... Q. .. &lt;

16030100 8610: reception of message Client_Key_Exchange.

6. CFTY02Z&gt;&gt; CTX = 210006 ndata () _ DATA RECEIVED FROM HANDSHAKE 6 NETWORK

CFTY02Z&gt;&gt; CTX = 14030100 210 006 0101 &gt;......&lt;

14030100 0101: Change_Cipher_Specs receiving the message.

7. CFTY02Z&gt;&gt; CTX = 210006 ndata () _ 53 RECEIVED FROM HANDSHAKE DATA NETWORK

CFTY02Z&gt;&gt; CTX = 210006 16030100 0C 304319F2 E3B4FC0E 1E4E8236 &gt;.... N.6 ....... &lt;

CFTY02Z&gt;&gt; CTX = 210006 DD323B1E 745E023A 31EEBDAB 2E271880&gt; .2 t. .. 1 .. .......&lt;

CFTY02Z&gt;&gt; CTX = 210006 AA2D9520 B4F43E0E D7ED0598 38275B5D &gt;............ 8 ... "

CFTY02Z&gt;&gt; CTX = 210006 74463CDE D8&gt; tF ... &lt;

16030100 3043: Finished the message was received.

8. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ 6 SENDING DATA HANDSHAKE

CFTY02Z&gt;&gt; CTX = 14030100 210 006 0101 &gt;......&lt;

14030100 0101: Sending message Change_Cipher_Specs.

9. CFTY02Z&gt;&gt; CTX = 210006 SSLact () _ 53 HANDSHAKE SENDING DATA

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
