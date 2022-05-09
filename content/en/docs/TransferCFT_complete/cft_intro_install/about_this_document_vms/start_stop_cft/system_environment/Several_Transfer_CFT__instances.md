{
    "title": "Multiple Transfer CFT instances on the same system",
    "linkTitle": "Multiple instances on the same system",
    "weight": "280"
}{{< TransferCFT/axwayvariablesComponentShortName  >}} is installed in a specific user account. Only one {{< TransferCFT/axwayvariablesComponentShortName  >}} can be active per user group on the same system.

If you want to execute several {{< TransferCFT/axwayvariablesComponentShortName  >}} instances on the same system, they must be executed in different groups. The UICs should have the following format, where AAA and BBB are different values: [AAA,XXX] and [BBB,YYY]

Otherwise, both {{< TransferCFT/axwayvariablesComponentShortName  >}}s overwrite the information stored in the first of the two global sections. This leads to inconsistent operating conditions.
