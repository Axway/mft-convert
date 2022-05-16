---

    title: Partners - CFTPART 
    linkTitle: Partners - CFTPART
    weight: 190

---
This page describes how to define the Partners object (CFTPART ) to set partner
characteristics. Use this command to describe partner characteristics.

See also:

- Command syntax
    [CFTPART](../../../command_summary#CFTPART)
- Object concepts
    [Defining partners]()


| Parameter  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/comment">COMMENT</a>  | Local comment associated with the partner.<br/> Not used during transfers. May be queried, for example by the LISTPART command. It can consequently be used to locally associate a long text (description in plain language) with the partner. |
| <a href="../../../command_summary/parameter_intro/commut">COMMUT</a> <br/> Only applicable for a store and forward site | Type of switching supported for this partner.<br/> The following types are possible:<br/> • YES: corresponds to "store and forward" switching<br/> • SERVER: corresponds to "VAN server switching"<br/> • NO: switching is refused for this partner<br/> • PART: store and forward mode is forced in server mode if the recipient specified in the IPART parameter is not the final receiver<br/> Processing according to the switching type is described in Transfer with routing - Store and forward. |
| <a href="">COS</a>  | Class of service parameter as it relates to bandwidth control.  |
| <a href="../../../command_summary/parameter_intro/ctrlpart">CTRLPART</a> | Optional parameter that is relevant only in server mode. When a transfer is initiated by a remote partner, Transfer CFT always controls its identity. If the remote sender is not the initial sender, store and forward mode, the Transfer CFT can choose to control the initial sender identity according to CTRLPART parameter.  |
| <a href="../../../command_summary/parameter_intro/fcharset">FCHARSET</a>  | Defines the local file encoding. See also <a href="../../../command_summary/parameter_intro/ncharset">ncharset.</a>  |
| <a href="../../../command_summary/parameter_intro/fprefix">FPREFIX</a> | File name.<br/> This parameter is used to associate a file name with a partner. During a send or receive operation (CFTSEND or SEND, CFTRECV or RECV), this file name is concatenated with the name specified in the FNAME parameter. |
| <a href="../../../command_summary/parameter_intro/group">GROUP</a>  | Group the partner belongs to.<br/> This parameter is used to locally define the symbolic variable &amp;GROUP. |
| <a href="../../../command_summary/parameter_intro/id">ID</a>  | Local partner identifier. |
| <a href="../../../command_summary/parameter_intro/idf">IDF</a>  | Default identifier of the file for the partner. |
| <a href="../../../command_summary/parameter_intro/imaxtime">IMAXTIME</a> | Maximum time after which the partner cannot call. |
| <a href="">IMINTIME</a>  | Minimum time before which the partner cannot call.<br/> IMINTIME and IMAXTIME define the time slot during which the partner calls the Transfer CFT monitor (incoming calls). |
| <a href="../../../command_summary/parameter_intro/ipart">IPART</a> | Local identifier of an intermediate partner. |
| <a href="../../../command_summary/parameter_intro/nack">NACK</a>  | PeSIT<br/> Enables or disables the negative acknowledgement feature. |
| <a href="../../../command_summary/parameter_intro/npart">NRPART</a> | Partner network identifier identifying the partner for incoming calls. |
| <a href="../../../command_summary/parameter_intro/nrpassw">NRPASSW</a>  | Partner sign-on password, authorizing a local site access right check. |
| <a href="../../../command_summary/parameter_intro/nspart">NSPART</a>  | Network identifier by which the local Transfer CFT monitor identifies itself to its partner. |
| <a href="../../../command_summary/parameter_intro/nspassw">NSPASSW</a> | Password by which the Transfer CFT monitor identifies itself to the partner. |
| <a href="../../../command_summary/parameter_intro/omaxtime">OMAXTIME</a> | Maximum time after which the partner can no longer be called. |
| <a href="../../../command_summary/parameter_intro/omintime">OMINTIME</a>  | Minimum time before which the partner may not be called.<br/> OMINTIME and OMAXTIME define the time slot during which the Transfer CFT can call this partner (outgoing calls).<br/> A time slot of zero means that the Transfer CFT monitor cannot call the partner directly; it then implements the store and forward mechanism by calling the intermediate site (IPART parameter). |
| <a href="../../../command_summary/parameter_intro/prot">PROT</a>  | List of communication protocols (CFTPROT ID identifiers) authorized to communicate with this partner. |
| <a href="../../../command_summary/parameter_intro/rauth">RAUTH</a>  | Identifier (ID parameter) of the CFTAUTH command designating a list of IDFs authorized for the receive transfers with this partner.<br/> RAUTH=* means that all the model files (all the IDFs) may be received.<br/> The identifier values beginning with the characters ‘NOT’ designate lists of unauthorized IDFs (see the CFTAUTH command). |
| <a href="../../../command_summary/parameter_intro/sap">SAP</a>  | Values of the remote SAPs, service access points, associated with each of the protocols defined by the PROT parameter. |
| <a href="../../../command_summary/parameter_intro/sauth">SAUTH</a>  | Identifier (ID parameter) of the CFTAUTH command designating a list of IDFs authorized for send transfers to this partner.<br/> SAUTH =* means that all model files (all IDFs) may be sent.<br/> The identifier values beginning with the characters ‘NOT’ designate lists of unauthorized IDFs (see CFTAUTH ). |
| <a href="">SSH</a>  | Associates a security profile with a partner definition.  |
| <a href="../../../command_summary/parameter_intro/ssl">SSL</a>  | The parameter used to associate a security profile with a partner definition.  |
| <a href="../../../command_summary/parameter_intro/state">STATE</a>  | Partner state. |
| <a href="../../../command_summary/parameter_intro/syst">SYST</a>  | Type of operating system supporting the partner.<br/> If this parameter is not defined, the partner is considered to have the same operating system as the local computer.<br/> This parameter allows the monitor:<br/> • To find the data code on the partner computer (see the comment for the FCODE parameter of the <a href="../../../../concepts/cft_configuration_concepts_start_here/translation_table_concepts">CFTXLATE</a> command)<br/> • To declare the file type, in this profile and when the partner is the receiver, in terms of its operating system, to the partner Transfer CFT (NTYPE parameter) |
| <a href="../../../command_summary/parameter_intro/trk">TRK</a> | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| <a href="../../../command_summary/parameter_intro/xlate">XLATE</a>  | Identifier of the translation table used for transfers with this partner.<br/> The translation table is defined by the CFTXLATE ID= &lt;xlate&gt; object.<br/> If this parameter is not defined, the translation tables may also be specified in the SEND/CFTSEND (or RECV/CFTRECV) commands. If these tables are not defined in these commands, the CFTXLATE command ID=&lt;default&gt; specifies them by default. If this command is not included for messages as well as for files, the translation tables that are internal to the monitor are used.<br/> See also <a href="../../../../concepts/cft_configuration_concepts_start_here/translation_table_concepts">Conversion tables: Translation</a>.  |


## CFTUTIL example

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
