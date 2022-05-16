---

    title: SFTP use case examples
    linkTitle: SFTP examples
    weight: 190

---
The supported operating systems are listed in the [Platform features](../../../datasheet) table.

## {{< TransferCFT/suitevariablesTransferCFTName  >}} acting as a SFTP client

****Put command****

```
CFTPART id=app1,nspart=login,nspassw=passw,prot=sftp,...
CFTTCP id=app1,host=host
send part=app1, idf=flow01, fname=localfiletosend, nfname=remotefile
```

The equivalent command for SFTP on Linux:

```
sftp login@host
sftp> put localfiletosend remotefile
```

****Get command****

```
recv part=app1, idf=flow01, fname=localfiletowrite, nfname=remotefile
 
The equivalent command for SFTP:
sftp> get remotefile localfiletowrite
```

****Mput command****

```
send part=app1,idf=groupoffiles,fname=(@/#)file\*
 
The equivalent command for SFTP:
sftp> mput file\*
```

****Mget command****

The {{< TransferCFT/axwayvariablesComponentLongName  >}} server must<span style="font-weight: normal;"> </span>be using open mode.

```
recv part=app1,idf=groupoffiles,nfname=(@/#)file\*,file=all
 
The equivalent command for SFTP:
sftp> mget file\*
```

## Transfer CFT client with a Transfer CFT server

This example sends an acknowledgment following a file transfer (<span class="code">`cft_flow`</span> in the example).

****On the Transfer CFT 1****

```
CFTPART ID=CFT_2_SFTP,nspart="cft_1_sftp",nspassw=cft_1_sftp,nrpart="cft_2_sftp",nrpassw=cft_2_sftp,sap=<CFT_2_SFTP_PORT>,prot=SFTP
CFTTCP ID=CFT_2_SFTP,host=<CFT_2_HOST>
```

****On {{< TransferCFT/axwayvariablesComponentLongName  >}} 2****

```
CFTPART ID=CFT_1_SFTP,nspart="cft_2_sftp",nspassw=cft_2_sftp,nrpart="cft_1_sftp",nrpassw=cft_1_sftp,sap=<CFT_1_SFTP_PORT>,prot=SFTP
CFTTCP ID=CFT_1_SFTP,host=<CFT_1_HOST>
```

****Execute the send on Transfer CFT 1****

```
send part=CFT_2_SFTP,idf=**cft_flow**,fname=localfiletosend,nfname=remotefile
```

****Execute the acknowledgment from Transfer CFT 2****

```
send part=CFT_1_SFTP,idm=cft_ack,type=reply, msg=completed, idt=&idt(of the **cft_flow**)
```

### Transfer CFT requester downloading multiple files

This example demonstrates receiving multiple files from a Transfer CFT SFTP server and is the equivalent of an <span class="code">`mget file*`</span>.

****On the Transfer CFT server****

If you do not define the workingdir, the default value is the runtime directory.

```
cftsend id=groupoffiles,impl=yes,fname=&nfname
```

****On {{< TransferCFT/axwayvariablesComponentLongName  >}} requester****

```
recv part=app1,idf=groupoffiles,nfname=(@/#)test/file\*,file=all
```

This results in downloading all remote files in the <span class="code">`test `</span>folder with the path relative to the workingdir.

## Transfer CFT client with a SecureTransport server

****On the Transfer CFT side****

```
CFTPART ID=ST_SFTP,nspart="st_sftp",nspassw=st_sftp,sap=<ST_SFTP_PORT>,prot=SFTP
CFTTCP ID=ST_SFTP,host=<ST_HOST>
```

****On the SecureTransport****

Server Control: the SSH server is running with **Enable Secure File Transfer Protocol (SFTP)**

```
Port=<ST_SFTP_PORT>
```

Accounts: the Account Name is <span class="code">`st_sftp Active`</span> with the Login <span class="code">`Name=st_sftp`</span> and <span class="code">`Password=st_sftp`</span>

```
send part=ST_SFTP,idf=st_flow,fname=localfiletosend,nfname=remotefile
```

## Transfer CFT server with multiple client keys

In this use case, the clients are using the key authentication method where the key is different for each client. This requires a separate partner definition and dedicated SSH profile for each user.

****On the Transfer CFT server side****

```
For each user define a CFTPART (in this example there are two users USER1 and USER2) as follows:
 
CFTPART ID=**USER1**,NRPART=USER1,SSH=USER1_SSH,PROT=SFTP,...
CFTSSH ID=USER1_SSH,DIRECT=SERVER,CLIPUBKEY=USER1_PUB, ...
 
CFTPART ID=**USER2**,NRPART=USER2,SSH=USER2_SSH,PROT=SFTP,...
CFTSSH ID=USER2_SSH,DIRECT=SERVER,CLIPUBKEY=USER2_PUB,...
 
CFTPROT ID=SFTP,TYPE=SFTP,SSH=SSH_DEFAULT,SAP=1763,...
 
CFTSSH ID=SSH_DEFAULT,SRVPRIVKEY=CFT_SSH_PRIV,CLIPUBKEY='',... (where '' indicates that the key is not set allowing multiple users)
 
To import the USER1_PUB and USER2_PUB keys, use the PKIUTIL command. For example, import the SSH keys as follows, where 'CFT' is the password value you entered in the CFTPARM object:
PKIUTIL PKIKEY ID=USER1_PUB, IKNAME=USER1_PUB.KEY, IKFORM=SSH
PKIUTIL PKIKEY ID=USER2_PUB, IKNAME=USER2_PUB.KEY, IKFORM=SSH
 
```

****For each of the various clients****

The client must log in as USER1 or USER2, in this example, and provide the corresponding private key as required for server connection.

## {{< TransferCFT/axwayvariablesComponentLongName  >}} transcoding when using {{< TransferCFT/PrimaryCGorUM  >}}

This example uses two {{< TransferCFT/axwayvariablesComponentLongName  >}} applications in {{< TransferCFT/PrimaryCGorUM  >}}, where the Source application is a UNIX machine and the Target is a z/OS system. On these applications, navigate to the indicated sections in the flow definition and define the following parameters to enable the transcoding.

****On the Source****

*File properties &gt; File encoding* &gt; File Type= Text , Encoding = ASCII , Transcoding = EBCDIC

****In the SFTP protocol****

*SFTP properties &gt;* Transfer mode = ASCII

****On the Target****

*File properties &gt; File encoding* &gt; File Type= Text , Encoding = EBCDIC , Transcoding = EBCDIC

****Related topics****

[CFTSSH - Security profile](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh)
