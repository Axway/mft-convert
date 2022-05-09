{
    "title": "Migrate and upgrade considerations",
    "linkTitle": "Migrate/upgrade considerations",
    "weight": "200"
}This section summarizes the migration and upgrade procedures for each type of object and provides related recommendations.

**Transfer CFT file IDs**


| File ID  | Migrate or upgrade procedures  |
| --- | --- |
| PARM  | MIGRPARM  |
| PART  | MIGRPART  |
| CATALOG  | MIGRCAT  |
| COM  | MIGRCOM  |
| LOG  | No migration step; you must create the file if you migrated.  |
| ACCNT  | No migration step; you must create the file if you migrated.<br/> For format=V24, some field lengths were modified, but the total size of record remains the same. |
| UCONF, unified configuration file  | MIGRUCNF.  |
| PKI file  | As of Transfer CFT 3.4, MIGRPKI was split into 2 procedures, MIGRPKI1 and MIGRPKI2.  |
| Exits/API  | The use of DLL mode is mandatory (otherwise you must rework the Exits and API). In any case, you must compile and link the exits and API.  |


**Recommendations**

- The delivered examples to create the files LOG and ACCOUNT (D40INIT..) have a `format=V24` definition. Ensure that the FORMAT parameter in the CFTLOG and CFTACCNT commands are consistent.
- As of Transfer CFT 3.1.3:
    -   A UCONFRUN file was added in JCL/STC/Procedures (CFTMAIN, COPRUN, PCFTUTIL, PCFTUTL, MNRMAIN, MNRMNG). We recommend adding the definition of this file in your JCL containing EXEC PGM=CFTUTIL, and in JCLs that are running Transfer CFT APIs.
    -   The product comes with components:

        -   CFTINC and CFTENV include JCL

        -   PCFTUTIL and PCFTUTL procedures

    -   Integrating these components where possible into your existing JCLs, simplifies migration.

- As of {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.3.2:
    -   SGINSTAL is optional and can be replaced by UCONF variables.
    -   A USER.LOAD library can contain API(s), EXIT(s), and SGINSTAL(optional). If you define a USER.LOAD, you must add this load in STEPLIB/JOBLIB in your JCL or STC, and it must be an APF.
