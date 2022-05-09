{
    "title": "Create  an authorized/unauthorized identifiers list",
    "linkTitle": "Creating authorizations lists CFTAUTH",
    "weight": "140"
}The CFTAUTH object can represent either a list of authorized or a list of unauthorized file identifiers for a partner.

List of authorized identifiers
------------------------------

You can define the list of authorized file identifiers for the CFTAUTH in one of the following two ways:

- IDF: Use a list of identifiers
    for the ****IDF**** parameter
- FNAME: Use a path to the file where
    the list is saved for the ****FNAME****
    parameter

These two methods are mutually exclusive; you cannot use the IDF and FNAME parameters simultaneously.

The following can be associated with each partner:

- Send list: The identifier of the send list is defined by the value of the SAUTH
    parameter of CFTPART.
- Receive list: The identifier of the receive list is defined by the value of the RAUTH
    parameter of CFTPART.

For an example and command parameter details, see [Authorized flow definitions - CFTAUTH](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftauth).

List of unauthorized identifiers
--------------------------------

To create a list of excluded IDFs, you simply prefix the authorization object (CFTAUTH) ID with the letters "`NOT`" and list the IDFs to exclude in the IDF parameter definition. For example, you can exclude some IDF from being sent to a partner using the SAUTH parameter.

****Example list of unauthorized IDFs****

With CFTAUTH ID = NOTSIBM1 in the CFTPART definition, and SAUTH = NOTSIBM1, then the APLI1 and APLI2 IDFs are not transferred to the IBM1 partner. When the CFTAUTH ID is prefixed by `NOT`, note that all other IDFs that are not included in the IDF list are consequently authorized.

```
CFTPART    ID = IBM1,
SAUTH =  NOT
SIBM1
CFTPART     ID = BULGC8,
RAUTH = RBULGC8
CFTAUTH     ID =  NOT
SIBM1,
IDF = (APLI1,APLI2)
CFTAUTH      ID = RBULGC8,
IDF = fil21
```

The log then displays the following message: `CFTT25E IDF not authorized <PART =% s IDF =% s>`

{{< TransferCFT/PrimaryCGorUM  >}} interoperability
--------------------------------------------------------

You cannot create, delete, or manage authorization template lists from the Central Governance UI. However, the list of authorized file identifiers, used to limit partner access to certain folders, is automatically managed in Central Governance for SFTP partners.
