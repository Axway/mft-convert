---
title: "Using SFTP with Transfer CFT"
linkTitle: "SFTP protocol"
weight: 110
---The SSH File Transfer Protocol (SFTP) is a protocol that transfers files over an encrypted SSH channel. The SSH protocol is an encrypted protocol that uses a client-server model to authenticate two parties and encrypt the data between them.

* The server listens on a designated port for connections, and is responsible for negotiating the secure connection, authenticating the connecting party, and spawning the correct environment if the credentials are accepted.

* The client begins the TCP handshake with the server, negotiates the secured connection, checks that the server's identity matches previously recorded information, and provides credentials to authenticate.

Please see [SSH session concepts](sftp_keys_concepts) for session concepts and SSH object details.

<span id="Supporte2"></span>

## Supported features    

The Transfer CFT supports the following SFTP features:

* Text/binary file transfer
* Group of files in heterogeneous mode (`mput, mget`)
* Folder monitoring
* Multi-node
* SSH compression
* Bandwidth control in client mode
* Authentication with the user password
* Authentication with an SSH key
* Dual authentication with user password and SSH key
* Amazon S3 and Ceph Storage
* Google Cloud Storage

<span id="Supporte"></span>

## Supported operations

The Transfer CFT in server mode supports the following SFTP commands:

* Upload (`put`) and download (`get`)
* Get directory listings
* Create, remove, change directory
* Rename, remove file
* Change file mode
* Action on remote source (delete or archive)

The Transfer CFT in client mode supports the following SFTP commands:

* Upload (`put`) and download (`get`)

## Supported SFTP versions

{{< TransferCFT/suitevariablesTransferCFTName  >}} supports the SFTP versions 3, 4, 5, and 6. Drafts 06–13 of the IETF Internet Draft define the successive revisions of SFTP version 6.

The {{< TransferCFT/axwayvariablesComponentLongName  >}} implementation is based on [Draft 13 of the RFC](https://datatracker.ietf.org/doc/html/draft-ietf-secsh-filexfer-13)  and supports the following [extensions](https://datatracker.ietf.org/doc/html/draft-ietf-secsh-filexfer-extensions-00): Vendor Id, File Hashing, Querying Available Space, Querying User Home Directory. However, {{< TransferCFT/axwayvariablesComponentLongName  >}} does not support the SFTP version 6 extended attributes and byte-range locks.

<span id="Use"></span>

## Use cases

You can use SFTP with parks of {{< TransferCFT/suitevariablesTransferCFTName  >}}, other Axway products, and third-party products, to connect file transfer networks.

### Use case 1: Simple integration

> **Note**
>
> Transfer CFT delivers a basic SFTP configuration template with the product that you can use to easily perform a first SFTP transfer. See the SFTP quick start!page for details on how to use this template to implement the following use case.

![](/Images/TransferCFT/sftp_UC1.png)

 

add use case in the cft client and sftp server put an application on the server use green for external blue for CFT, green for external, remove versions...add another instance cft that uses native files - add application group openvms goes directly to ST using PeSIT SSL

### Use case 2: Simple integration

 

### Use case 3: Connecting networks

Transfer CFT can transfer files between applications using PeSIT SSL or SFTP, as either a client or a server.

 

arrows in both directions.

![](/Images/TransferCFT/temp_111.png)

> ![](/Images/TransferCFT/sftp_UC2.png)

<span id="Limitati"></span>

## Limitations

* SFTP supports messages and replies (ACK or NACK), but only between Transfer CFTs.
* SFTP implementation does not support the store-and-forward functionality.
* You cannot use Copilot to create CFTSSH objects.
* When using Secure Relay with Transfer CFT for SFTP exchanges, SSH termination is not supported.
* Transfer CFT supports the RSA digital signature algorithm; however, ECDSA and DSA are not supported.
* 512-bit RSA keys are not supported. Use at least a 1024-bit key for RSA.
* *Windows* - You cannot modify the files rights (chmod) from the SFTP client when using Transfer CFT Windows as the SFTP server.
* *z/OS* - Only z/OS UNIX files are processed, not native files.
* *IBM i* - Only IBM i UNIX files are processed, not native files.
* *HP NonStop* - Only UNIX files are processed, not native files.
* Not supported on OpenVMS systems.

Limitations when using Amazon S3, Ceph, or Google Cloud Storage with SFTP (*available on Unix and Windows systems*):

* You cannot restart a transfer in server receive mode.
* When listing files, only the size and modification time display.
* You cannot see how much free space is available in the Cloud folder.
