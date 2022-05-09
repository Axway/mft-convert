{
    "title": "About  the communication area",
    "linkTitle": "About the communication area",
    "weight": "330"
}This topic describes the communication area structure. The communication
area contains:

- [General
    information fields](#General_information_fields)
- [Return
    codes](#Return_codes___directory_exit)
- [Partner
    information](#Partner_information___directory_exit)
- [Partner
    network information](#Partner_network_information___directory_exit)
- [Information
    per network type](#Partner_network_information___directory_exit) (not supported by all networks)

The next topic describes the communication area structure between the
interface and the user program.

The following tables list all the fields of the communication structure.

<span id="General_information_fields"></span>

### General information fields


| ****Field**** | ****Explanation**** |
| --- | --- |
| version  | Interface version number<br/> The current version number is &quot;0130&quot;. |
| idexit  | EXIT task identifier |
| exname | User name<br/> The user name is equal to the value of the exaref parameter of the exaini function. |
| parm | User parameter<br/> The user parameter is equal to the value of the PARM parameter of the CFTEXIT object. |
| exver | Exit version V24 or higher |
| language | Language of the user program:<br/> • C: C language<br/> • O: COBOL |
| etype | EXIT task type<br/> A: directory Exit |
| etrace | Inoperative parameter |
| mode | Call mode:<br/> • R: Requester mode<br/> • S: Server mode |
| commutfl | Switching flag (intermediate partner):<br/> • 0: no switching in process<br/> • 1: switching in process |
| curtime | Current time (HHMMSSCC) |
| curdate | Current date (YYYYMMDD) |
| curreqc | Current number of transfers in requester mode for the given network partner |
| cursrvc | Current number of transfers in server mode for the given network partner |
| maxrty | Requester mode only<br/> Maximum number of network connection attempts |
| currty | Requester mode only<br/> Current number of network connection attempts |
| maxrtyp  | Requester mode only<br/> Maximum number of protocol connection attempts |
| currtyp | Requester mode only<br/> Current number of protocol connection attempts |
| protl  | Information about the protocols of the CFTPARM object:<br/> • protocol identifier<br/> • protocol type<br/> • profile, in the case of the PESIT protocol<br/> • associated network type |
| idf  | Partner-related file identifier |


<span id="Return_codes___directory_exit"></span>

### Return codes


| Field | Explanation |
| --- | --- |
| ret1 | Return code<br/> • 0: Processing ok<br/> • 1: Transfer CFT must take charge of the call<br/> • 2: Connection refusal<br/> • 9: Processing error |
| ret2  | Cause of connection refusal if the return code value is 2<br/> • 1: Partner not known<br/> • 2: Password not valid<br/> • 3: Address not known<br/> • 4: Call time not valid<br/> • 5: Communication protocol not valid<br/> • 6: Maximum number of virtual circuits reached<br/> • 7: Maximum number of transfers reached<br/> • 8: Maximum number of partners reached<br/> • 9: Caller taxation refusal<br/> • 10: Network problem<br/> • 255: Other cause of connection refusal<br/> These refusal codes are converted by {{< TransferCFT/axwayvariablesComponentShortName  >}} into internal diagnostic codes.<br/> In server mode, the internal diagnostic codes are converted into protocol diagnostic codes and sent to the calling partner.<br/> In requester mode, the internal diagnostic codes are saved in the catalog. |
| diag | Diagnostic code if the return code is not 0 and 1<br/> The contents of this field are used in the EXIT related error. |
| msg  | User message<br/> If this field is defined, the content is sent to the Transfer CFT standard output. |


<span id="Partner_information___directory_exit"></span>

### Partner information


| Field | Explanation |
| --- | --- |
| ptype | Partner type:<br/> • C: normal {{< TransferCFT/axwayvariablesComponentShortName  >}} partner<br/> • D: dynamic {{< TransferCFT/axwayvariablesComponentShortName  >}} partner<br/> • E: non {{< TransferCFT/axwayvariablesComponentShortName  >}} partner |
| part | Partner local identifie |
| ipart | Intermediate partner local identifier |
| cpart | Associated store and forward partner identifier |
| idnet | Network resource identifier |
| idprot | Protocol identifier |
| prot | Protocol type:<br/> • P: PeSIT protocol<br/> • 0: ODETTE protocol |
| prof | PeSIT only<br/> Profile:<br/> • C : PeSIT D, Cft<br/> • S: PeSIT D, Sit<br/> • A: PeSIT E<br/> • D: PeSIT E (DMZ) |
| syst | Operating system:<br/> • 4: OS400<br/> • 7: GCOS7<br/> • 8: GCOS8<br/> • M: MVS<br/> • S: UNIX<br/> • V: VMS<br/> • G: GUARD<br/> • B: BS2000<br/> • W: WINDOWS |
| code  | Code:<br/> • A: ASCII<br/> • E: EBCDIC |
| open | No longer supported |
| commut | Store and forward indicator:<br/> • N: No store and forward<br/> • Y: Immediate store and forward<br/> • S: Deferred store and forward<br/> • P: Forced commutation |
| sap | Remote SAP (Service Access Point) |
| imaxtime | Maximum incoming call time (HHMMSSCC) |
| imintime | Minimum incoming call time (HHMMSSCC) |
| omaxtime | Maximum outgoing call time (HHMMSSCC) |
| omintime | Minimum outgoing call time (HHMMSSCC) |
| nrpart | Remote partner name |
| nrpassw | Remote partner password |
| newnrpw | PeSIT E only<br/> Remote partner new password |
| nspart | Local name |
| nspassw | Local password |
| newnspw | PeSIT E only<br/> New local password |
| group | Group identifier |
| sauth | File send authorization list identifier |
| rauth | File receive authorization list identifier |
| xlate | Transcoding table identifier |


<span id="Partner_network_information___directory_exit"></span>

### Partner network information


| Field | Explanation |
| --- | --- |
| ntype | Network type:<br/> • T: TCP |
| addr | Remote partner address |
| imaxt | Maximum incoming call time on the network resource |
| imint | Minimum incoming call time on the network resource |
| omaxt  | Maximum outgoing call time on the network resource |
| omint | Minimum outgoing call time on the network resource |
| retryw | Spacing of connection attempts (in minutes) |
| retryn | Number of connection attempts spaced by retryw |
| retrym | Maximum number of connection attempts |
| cnxin | Maximum number of simultaneous incoming calls for the given network partner |
| cnxout | Maximum number of simultaneous outgoing calls, for the given network partner |
| cnxinout | Maximum number of simultaneous communications, for the given network partner |


<span id="Network_dependent_information___directory_exit"></span>

### Network dependent information


| Field | Explanation |
| --- | --- |
| udata | User data |
| odata | Other data. |
| speedin | Virtual circuit input speed (in bits/second) |
| speedout | Virtual circuit output speed (in bits/second) |
| pcvin | Incoming reverse charge call:<br/> • 1: Yes<br/> • 0: No |
| pcvout | Outgoing reverse charge call:<br/> • 1: Yes<br/> • 0: No |
| gfa | Closed subscriber group number |
| verify | Number of characters (between 0 and 64) to be checked in the caing partner address |
| parity | Obsolete parameter |
| databit | Obsolete parameter |
| stopbit | Obsolete parameter |
| modout | Obsolete parameter |
| config | Obsolete parameter |
| padno | Obsolete parameter |
| padset | Obsolete parameter |


### Additional information


| Field | Explanation |
| --- | --- |
| cMode | SSL mode Client/Server |
| cAuthPolicy | SSL auth Anonymous/Simple/Double |
| bCipher | SSL cipher suite |
| sParm | SSL command free parameters |
| sRemoteUserDn | Remote User certificate Dn |
| sRemoteIssuerDn | Remote Issuer Dn |
| sRemoteCaId | Remote CA Alias |
| sUserCId | User Certificate Alias |
| sCertFname | File including Remote certificate |
| sProf | SSL profile ID |
| sRemoteSerial | Serial Number |
| ExitFree | Free Area between all EXITs |
| XferCycleId | CycleId for tracking occurrences |
| XferObjectcId | Name of the transfer tracking class |

