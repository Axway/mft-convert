{
    "title": "Transfer synchronization",
    "linkTitle": "Transfer synchronization",
    "weight": "280"
}Transfer CFT provides the following synchronization services:

- Set synchronization points during transfer
- Resume an interrupted transfer from a synchronization point
- Dynamically resynchronization during transfer

Services
--------

### Set synchronization points

The partner sending the file data can mark the transfer by means of sequentially numbered synchronization points. The receiving partner acknowledges a synchronization point to indicate the correct reception of the data sent up to that point. The synchronization points and their frequency and the acknowledgement window, are options which can be negotiated between the two partners at the time the logical connection is established.

### Resume a transfer

This function lets the requesting user resume a transfer that was interrupted before completion. In case of incident, the synchronization points mechanism allows a transfer to be resumed from a point other than the beginning of the file. It is possible to resume both a write transfer and a read transfer.

A transfer may be interrupted by an error, such as a network issue, or as a result of an operator action like the HALT command. At the requester end, the transfer may be reactivated manually (CFT START command) or, in certain cases, automatically. The Transfer CFT authorizes the transfer to restart at the last synchronization point set before interruption. If both partners do not succeed in resynchronizing with each other, the transfer begins again from the start of the file.

### Dynamically resynchronize

This function allows the exchange to be resumed from the last synchronization point acknowledged, in case of a remediable incident occurring during data transfer, in particular in the event of a transmission error being detected. Unlike transfer resumption, this mechanism is involved when the transfer has not been interrupted.

PeSIT synchronization
---------------------

{{% TransferCFT/snippets/synchronization_options%}}

SFTP synchronization
--------------------

### Restart a transfer

Transfers are activated by the client, so a restart only works from the client side. Between two Transfer CFTs, interrupted transfers are restarted as on other protocols. Note that:

- There is only one entry in the catalog for the transfer.
- You should use the file name as the transfer identifier during the restart.

When a send is restarted by the client, Transfer CFT checks that the file is still available on the server. This is possible only when the transfer is configured in Open Mode, as otherwise the client does not know the server's name.

### Configuration parameters

Specify the following parameters:

- rpacing: when CFT is receiving a non negotiated, internal file, the size in Kbytes between two synchronization points for the receiver. 0= no synchronization point and the default is 32,000 (32 MB).
- spacing: when CFT is sending the non negotiated internal file, the size in Kbytes, between two synchronization points for the sender. 0= no synchronization point and the default is 32,000 (32 MB).
- srusize:When sending a file, the block size. Note that the larger the block size the more you optimize the I/O process with a maximum 32,700 bytes.
- rrusize: When receiving a file, the block size. Note that the larger the block size the more you optimize the I/O process with a maximum 32,700 bytes.

****Example****

`CFTPROT  ID = SFTP,         ...           SPACING = 32000,         ...`

OFTP synchronization
--------------------

{{% TransferCFT/snippets/oftp_sync%}}

****Example****

`CFTPROT  ID = OFTP,         ...         SCREDIT   = 4,           SRUSIZE = 20000,         ...`

The sender is able to send up to 80,000 bytes (4 x 20000) before each synchronization
point set.

PeSIT synchronization PIs
-------------------------

{{% TransferCFT/snippets/PI07%}}
{{% TransferCFT/snippets/PI18%}}
{{% TransferCFT/snippets/PI20%}}
{{% TransferCFT/snippets/PI23%}}
