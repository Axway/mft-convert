{
    "title": "Authorized flow definitions - CFTAUTH  ",
    "linkTitle": "CFTAUTH - Authorized templates",
    "weight": "280"
}<span id="Listing_model_file_identifiers__IDF_"></span>This section describes
the CFTAUTH command and parameters you use to create lists of authorized or unauthorized file transfer identifiers. For each CFTAUTH object, you can define either an FNAME or an IDF using the following syntax:

```
CFTAUTH     ID = <authorization_list_1>,
IDF = <file ID>
CFTAUTH     ID = <authorization_list_2>,
FNAME = <filename>
```

****Related
topics****

Object concepts
[](../../../../concepts/cft_configuration_concepts_start_here/authorization_list_concepts)[Create
authorized/unauthorized identifiers list](../../../../concepts/cft_configuration_concepts_start_here/authorization_list_concepts)


| Parameter  | Description  |
| --- | --- |
| [ID](../../../command_summary/parameter_intro/id) | Authorization list identifier. If the identifier is prefixed by&quot;<code>NOT</code>&quot;, the object indicates a list of forbidden file identifiers. For example, CFTAUTH ID = NOTSIBM1. |
| [FNAME](../../../command_summary/parameter_intro/fname) | The name of the file where authorized or unauthorized file identifiers (IDF) are listed.<br/> Each element of the list in this file can be:<br/> • An explicit file identifier, or<br/> • A mask (using wildcards '*?'), where all of the file identifiers corresponding to this mask are affected<br/> There is no limit to the number of identifiers in this list. |
| [IDF](../../../command_summary/parameter_intro/idf) | List of authorized or unauthorized IDFs.<br/> The value associated with each of these IDFs may be:<br/> • An explicit file identifier, or<br/> • A mask (using wildcards '*?'), where all of the file identifiers corresponding to this mask are affected by the command |


****<span id="CFTAUTH_example"></span>Example****

In this example, the local Transfer CFT can only send files with identifiers of the APLI1, APLI2, LIST type and files with
identifiers beginning with the 3 letters CHQ to the partner
IBM1.

```
CFTPART    ID = IBM1,
SAUTH = SIBM1
CFTPART     ID = BULGC8,
RAUTH = RBULGC8
CFTAUTH     ID = SIBM1,
IDF = (APLI1,APLI2,LIST,CHQ\*)
CFTAUTH      ID = RBULGC8,
IDF = fil21
```

Transfer CFT can only receive BULGC8 files with identifiers
of the idfdef, idf1, idf2, idf3 type from the partner.

Where the fil21 file contains:

- idfdef
- idf1
- idf2
- idf3

Please refer to [Create
authorized/unauthorized identifiers list](../../../../concepts/cft_configuration_concepts_start_here/authorization_list_concepts) for an example of how to define a list of unauthorized (excluded) IDFs.
