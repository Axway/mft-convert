{
    "title": "Partners - CFTPART ",
    "linkTitle": "CFTPART - Defining partners",
    "weight": "390"
}This page describes how to define the Partners object (CFTPART ) to set partner
characteristics. Use this command to describe partner characteristics.

See also:

- Command syntax
    [CFTPART](../../../command_summary#CFTPART)
- Object concepts
    [Defining partners]()


| Parameter  | Description  |
| --- | --- |
| [COMMENT](../../../command_summary/parameter_intro/comment)  | Local comment associated with the partner.<br/> Not used during transfers. May be queried, for example by the LISTPART command. It can consequently be used to locally associate a long text (description in plain language) with the partner. |
| [COMMUT](../../../command_summary/parameter_intro/commut) <br/> Only applicable for a store and forward site | Type of switching supported for this partner.<br/> The following types are possible:<br/> • YES: corresponds to &quot;store and forward&quot; switching<br/> • SERVER: corresponds to &quot;VAN server switching&quot;<br/> • NO: switching is refused for this partner<br/> • PART: store and forward mode is forced in server mode if the recipient specified in the IPART parameter is not the final receiver<br/> Processing according to the switching type is described in Transfer with routing - Store and forward. |
| [COS]()  | Class of service parameter as it relates to bandwidth control.  |
| [CTRLPART](../../../command_summary/parameter_intro/ctrlpart) | Optional parameter that is relevant only in server mode. When a transfer is initiated by a remote partner, Transfer CFT always controls its identity. If the remote sender is not the initial sender, store and forward mode, the Transfer CFT can choose to control the initial sender identity according to CTRLPART parameter.  |
| [ncharset.](../../../command_summary/parameter_intro/fcharset)  |
| [FPREFIX](../../../command_summary/parameter_intro/fprefix) | File name.<br/> This parameter is used to associate a file name with a partner. During a send or receive operation (CFTSEND or SEND, CFTRECV or RECV), this file name is concatenated with the name specified in the FNAME parameter. |
| [GROUP](../../../command_summary/parameter_intro/group)  | Group the partner belongs to.<br/> This parameter is used to locally define the symbolic variable &amp;GROUP. |
| [ID](../../../command_summary/parameter_intro/id)  | Local partner identifier. |
| [IDF](../../../command_summary/parameter_intro/idf)  | Default identifier of the file for the partner. |
| [IMAXTIME](../../../command_summary/parameter_intro/imaxtime) | Maximum time after which the partner cannot call. |
| [IMINTIME]()  | Minimum time before which the partner cannot call.<br/> IMINTIME and IMAXTIME define the time slot during which the partner calls the Transfer CFT monitor (incoming calls). |
| [IPART](../../../command_summary/parameter_intro/ipart) | Local identifier of an intermediate partner. |
| [NACK](../../../command_summary/parameter_intro/nack)  | PeSIT<br/> Enables or disables the negative acknowledgement feature. |
| [NRPART](../../../command_summary/parameter_intro/npart) | Partner network identifier identifying the partner for incoming calls. |
| [NRPASSW](../../../command_summary/parameter_intro/nrpassw)  | Partner sign-on password, authorizing a local site access right check. |
| [NSPART](../../../command_summary/parameter_intro/nspart)  | Network identifier by which the local Transfer CFT monitor identifies itself to its partner. |
| [NSPASSW](../../../command_summary/parameter_intro/nspassw) | Password by which the Transfer CFT monitor identifies itself to the partner. |
| [OMAXTIME](../../../command_summary/parameter_intro/omaxtime) | Maximum time after which the partner can no longer be called. |
| [OMINTIME](../../../command_summary/parameter_intro/omintime)  | Minimum time before which the partner may not be called.<br/> OMINTIME and OMAXTIME define the time slot during which the Transfer CFT can call this partner (outgoing calls).<br/> A time slot of zero means that the Transfer CFT monitor cannot call the partner directly; it then implements the store and forward mechanism by calling the intermediate site (IPART parameter). |
| [PROT](../../../command_summary/parameter_intro/prot)  | List of communication protocols (CFTPROT ID identifiers) authorized to communicate with this partner. |
| [RAUTH](../../../command_summary/parameter_intro/rauth)  | Identifier (ID parameter) of the CFTAUTH command designating a list of IDFs authorized for the receive transfers with this partner.<br/> RAUTH=* means that all the model files (all the IDFs) may be received.<br/> The identifier values beginning with the characters ‘NOT’ designate lists of unauthorized IDFs (see the CFTAUTH command). |
| [SAP](../../../command_summary/parameter_intro/sap)  | Values of the remote SAPs, service access points, associated with each of the protocols defined by the PROT parameter. |
| [SAUTH](../../../command_summary/parameter_intro/sauth)  | Identifier (ID parameter) of the CFTAUTH command designating a list of IDFs authorized for send transfers to this partner.<br/> SAUTH =* means that all model files (all IDFs) may be sent.<br/> The identifier values beginning with the characters ‘NOT’ designate lists of unauthorized IDFs (see CFTAUTH ). |
| [SSH]()  | Associates a security profile with a partner definition.  |
| [SSL](../../../command_summary/parameter_intro/ssl)  | The parameter used to associate a security profile with a partner definition.  |
| [STATE](../../../command_summary/parameter_intro/state)  | Partner state. |
| [CFTXLATE](../../../command_summary/parameter_intro/syst) command)<br/> • To declare the file type, in this profile and when the partner is the receiver, in terms of its operating system, to the partner Transfer CFT (NTYPE parameter) |
| [TRK](../../../command_summary/parameter_intro/trk) | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| [Conversion tables: Translation](../../../command_summary/parameter_intro/xlate).  |


CFTUTIL example
---------------

```
CFTPART     MODE =    
CREATE,
     ID =     PARIS5,    
/\* Partner identifier      \*/
     PROT =     PSITCFT,    
/\* Only one communication protocol
                                   
-> see CFTPROT      \*/
     SAP =     13,          
/\*      \*/
     RAUTH =     RECPAR5,    
/\* The files authorized to be received
                                     
--> see CFTAUTH      \*/
     NRPART =     BULLDPS,    
/\* Name and password that the     \*/
     NRPASSW =     44NTS,    
/\* partner submits at connection time  \*/
     NSPART =     LOCALCFT,/\*
Name and password that CFT     \*/
     NSPASSW =     75P015,   
/\* submits to the partner on connection \*/
     IMINTIME =     0700,      /\*
Only able to call between     \*/
     IMAXTIME =     0900,        /\*
7:00 and 9:00     \*/
     OMINTIME =     1000,          /\*
May only be called between      \*/
     OMAXTIME =     1200    
,          /\* 10:00
and midday      \*/
```

The SAUTH parameter is omitted. The partner can then send any IDF to
the local {{< TransferCFT/axwayvariablesComponentShortName  >}}, while it is only authorized to receive
the files which have been assigned an IDF included in the RECPAR5 list
defined by a CFTAUTH object.

As the SYST parameter is not specified, {{< TransferCFT/axwayvariablesComponentShortName  >}} considers that its
partner is the same computer. The data code is consequently not converted
and the file types sent are not translated.
