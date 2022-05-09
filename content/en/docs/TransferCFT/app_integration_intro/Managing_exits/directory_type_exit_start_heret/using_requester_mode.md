{
    "title": "Using requester mode",
    "linkTitle": "Using requester mode",
    "weight": "400"
}The communication structure is defined by the interface before
the user function is called. You must provide, complete, or modify the
parameters that {{< TransferCFT/axwayvariablesComponentShortName  >}} needs to establish network and protocol connections.

The initialization function and the user function, if needed, are called
when connection requests are made, even for network and protocol connection
attempts (retries) or store and forward operations.

If the partner is:

- Known to Transfer
    CFT: the communication structure fields indicate the values of the CFTPART
    and CFTTCP corresponding to the partner
- Not known to Transfer
    CFT: the only fields containing valid data are the general information
    fields and the fields contained in the following table (partner information
    fields)

### Partner information fields


| Field  | Explanation  |
| --- | --- |
| Field  | Explanation  |
| ptype  | Partner type  |
| part  | Partner local identifier  |
| idne  | Network resource identifier  |
| idprot  | Protocol identifier  |
| prof  | PeSIT only<br/> Profile |
| prot  | Protocol type  |
| commut  | Store and forward indicator (‘Y’)  |
| imaxtime  | Maximum incoming call time (&quot;23595999&quot;)  |
| imintime  | Minimum incoming call time (&quot;00000000&quot;)  |
| omaxtime  | Maximum outgoing call time (&quot;23595999&quot;)  |
| omintime  | Minimum outgoing call time (&quot;00000000&quot;)  |
| ntype  | Network type  |
| imaxt  | Maximum incoming call time on the network resource (&quot;23595999&quot;)  |
| imint  | Minimum incoming call time on the network resource (&quot;00000000&quot;)  |
| omaxt  | Maximum outgoing call time on the network resource (&quot;23595999&quot;)  |
| omint  | Minimum outgoing call time on the network resource (&quot;00000000&quot;)  |
| retryw  | Spacing of connection attempts (7 minutes)  |
| retryn  | Number of connection attempts spaced by retryw (6)  |
| retrym  | Maximum number of connection attempts (12)  |
| cnxin  | Maximum number of simultaneous incoming calls, for the given network partner (2)  |
| cnxout  | Maximum number of simultaneous outgoing calls, for the given network partner (2)  |
| cnxinout  | Maximum number of simultaneous communications, for the given network partner (2)  |
| cMode | SSL mode Client/Server |
| cAuthPolicy | SSL auth Anonymous/Simple/Double |
| bCipher | SSL cipher suite |
| sParm | SSL command free parameters |
| sRemoteUserDn | Remote User certificate Dn |
| sRemoteIssuerDn | Remote Issuer Dn |
| sRemoteCaId | Remote CA Alias |
| sUserCId | User Certificate Alias |
| sCertFname | File including Remote certificate |
| sProf | SSL profil Id. |
| sRemoteSerial | Serial Number |
| ExitFree | Free Area between all EXITs |


 

When {{< TransferCFT/axwayvariablesComponentShortName  >}} does not know the partner, the following fields are
empty:

- ipart: intermediate
    partner local identifier
- sap: remote Sap
    (Service Access Point)
- addr: remote partner
    address

The ****ret1**** return code field must
be defined when the user function is returned from.

If there is a connection refusal (return code value of 2), the ret2
field may be defined to make the cause of the refusal appear in the DIAGI
field of the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog.

The content of the diag field appears with the appropriate error message
if the return code is not 0 and 1.

If the msg field is defined, its content is sent to the {{< TransferCFT/axwayvariablesComponentShortName  >}}
standard output.

If the return code value is 0 or 1, you can modify all the fields
except the general information fields or the ptype and ntype fields.

****Field descriptions****


| Field  | Explanation  |
| --- | --- |
| part  | If this field:<br/> • is empty when the user function is returned from, the partner local identifier &quot;UNDEFPTN&quot; appears in the catalog and on the {{< TransferCFT/axwayvariablesComponentShortName  >}} standard output.<br/> • has been modified and if the new identifier is located in the {{< TransferCFT/axwayvariablesComponentShortName  >}} partner base, {{< TransferCFT/axwayvariablesComponentShortName  >}} sets the ret1 field to 9 (processing error) and the diag field to &quot;PTNEXIST&quot;<br/> • has been modified, during any network or protocol connection attempts that may have been made, the system behaves as if the partner is not known to Transfer CFT |
| ipart  | If the ipart field is defined, a voluntary store and forward or backup mechanism<br /> (omintime = omaxtime) is possible.<br /> The user function is called to provide complete information or modify the intermediate partner information<br /> In the store and forward case, the commutfl field is set to 1. |
| idprot, idnet, prot and prof  | The idnet, prot and prof fields are connected to the CFTPROT command identifier (idprot). If the idprot field is modified, the new value must correspond to a CFTPROT command ( {{< TransferCFT/axwayvariablesComponentShortName  >}} updates the fields that are associated with it)<br /> If not, {{< TransferCFT/axwayvariablesComponentShortName  >}} sets the ret1 field to 9 (processing error) and the diag field to &quot;NOPROT&quot;.<br /> <br/> The protl field indicates the list of {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter setting protocols.  |
| nspart, nspassw, sap and addr  | If the nspart (and nspassw as applicable), sap and addr fields are not defined when the user function is returned from, the network and protocol connections will not be able to be established<br /> <br /> The maximum length taken into account for the remote partner address depends on the network type:<br/> • TCP: 64<br/> |
| nrpart  | If the field nrpart is empty when the user function is returned from, it is set to the value of the part field.<br/> A network partner is identified by a network name (nrpart).<br/> If the user establishes simultaneous connections with several network partners using the same name, the values of the counters below will be the sum of these connections and will be the same for each partner:<br/> • curreqc: current number of transfers in requester mode, for the given network partner<br/> • cursrvc: current number of transfers in server mode, for the given network<br /> partner |
| maxrty<br /> and currty  | During network connection attempts, the currty value may be used to assign a new address to the remote partner<br /> This counter is reset to zero each time the remote partner address is changed.<br/> If the currty value reaches the maxrty value, Transfer CFT attempts to go on to the next backup address.<br /> If there are no further backup addresses, {{< TransferCFT/axwayvariablesComponentShortName  >}} attempts to go on to the next backup protocol. If there are no further backup protocols, {{< TransferCFT/axwayvariablesComponentShortName  >}} attempts to perform a backup store and forward<br/> With the directory type EXIT task in requester mode, and one and only one protocol and a single address associated with a partner, {{< TransferCFT/axwayvariablesComponentShortName  >}} attempts the backup store and forward. If the intermediate partner does not exist, the transfer changes to the K state. |
| maxrtyp and currtyp  | During protocol connection attempts, the currtyp value may be used to assign a new protocol to the remote partner<br /> The counter is reset to zero each time the protocol is changed<br/> If the currtyp value reaches the maxrtp value, Transfer CFT attempts to go on to the next backup protocol<br /> If there are no further backup protocol, the transfer changes to the K state<br/> With the directory type EXIT in requester mode, and a single protocol associated with the partner, the transfer changes to the K state  |


During network and protocol connection attempts, the fields that can
be modified and whose values are kept relative to the last call of the
user function are:

- part: partner local
    identifier
- idprot: protocol
    identifier
- nrpart: remote
    partner name

Users can query an electronic directory (X500, DNS, ...) or their own
bases to locate their information (network name, password, remote sap,
remote partner address, and so on).
