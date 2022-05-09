{
    "title": "Migration procedure",
    "linkTitle": "Migrate ",
    "weight": "240"
}This section describes the Transfer CFT z/OS migration procedure and the statements you use to complete the process.

Prerequisites
-------------

Before you start, create a new Transfer CFT instance following the [installation procedure](../../overview_install_zos) in this guide. This is your target instance.

Overview
--------

Migration consists of extracting the configuration (CFTPARM, CFTPART, UCONF, PKI), the contents of the catalogs, the communication media files from the source instance (old installation), and importing them into the target instance (new installation).

All migration operations are from the target instance.

The following table lists and describes the MIGR\* members used in a Transfer CFT migration process. If you want to use the delivered procedure, as a first step you need to customize the MIGR$SET member.

Migration Job Control Language (JCL) Statements


| Member name | Purpose |
| --- | --- |
| MIGR$SET | JCL variables to customize |
| MIGRCAT | Migrate the catalog file |
| MIGRCOM | Migrate the communication file |
| MIGRPARM | Migrate the Managed File Transfer parameters file |
| MIGRPART | Migrate the Managed File Transfer partners file |
| MIGRPKI1 | Export the keys and certificates |
| MIGRPKI2 | Import the keys and certificates |
| MIGRPTIN | Sample - Include JCL called by MIGRPART |
| MIGRPTCL | Sample - CLIST updates partners extract file |
| MIGRPMIN | Sample - Include JCL called by MIGRPARM |
| MIGRPMCL | Sample - CLIST updates parameters extract file |
| PMIGR2 | Common procedure to migrate PARM, PART, CATALOG and communication media file. |


The MIGR$$$ file is located in the target.INSTALL library, and contains information about the JCL required for a Transfer CFT z/OS migration. Among the delivered JCL, MIGR$SET is used to customize variables used in the migration procedure. See the Migration Job Control Language (JCL) Statements table below for a description of the JCL and members to use.

The PMIGR2 procedure, is comprised of several steps:

- Export the content of the source file in a sequential work file.
- Optionally, activate a step to adapt this extracted file (Only for PART and PARM file)
- Create (recreate) the target file (or no operation).
- Import from the work file the data in the target file.
- List the content of target file.

Procedure
---------

This section describes how to migrate the various configuration elements in a non-multi-node environment or in a multi-node environment. Except for the migration of catalogs and the media communication file in a multi node environment, which are described in a specific section *Procedure for mutli-node*.

1. Customize the MIGR$SET file.  
    Edit the MIGR$SET and SET variables.
1. Migrate the PARM file (MIGRPARM).

The following variables can be set in MIGR$SET file or/and in the PMIGR2 parameters.


| Variable | Default value | Definition |
| --- | --- | --- |
| OLDEXEC |   | Executable library for the Transfer CFT version (source) to migrate. |
| OLDPARM |   | Transfer CFT PARM source file name. |
| NEWPARM |   | Transfer CFT PARM target file name. |
| DISPPARM | ‘R’ | 'R' for replace, 'A' for ADD:<br/> ‘R’ = Creates or recreates the Transfer CFT PARM file.<br/> ‘A’ = Only creates the Transfer CFT PARM file. |
| TMPPARM | &amp;CFTENV..MPARM | Work file name. |
| TMPSPARM | 'CYL,(10,2)' | Size allocation for work file. |
| CUSTOMPM | DUMMYJ<br/> (No customization) | Include member to customize parameters extract file. |


****Submit the procedure ..INSTALL(MIGRPARM)****

1. Migrate the PART file. (MIGRPART)

The following variables can be set in MIGR$SET file or/and in the PMIGR2 parameters.


| Variable | Default value | Definition |
| --- | --- | --- |
| OLDPART |   | Transfer CFT PART source file name. |
| NEWPART |   | Transfer CFT PART target file name. |
| DISPPART | 'R' | 'R' for replace, 'A' for ADD:<br/> ‘R’ = Create or recreate Transfer CFT PART file.<br/> ‘A’ = Only creates the Transfer CFT PART file. |
| TMPPART | &amp;CFTENV..MPARM | Work file name. |
| TMPSPART | 'CYL,(30,5)' | Size allocation for work file. |
| CUSTOMPT | DUMMYJ<br/> (No customization) | Include member to customize partners extract file. |


****Submit the procedure ..INSTALL(MIGRPART)****

1. Migrate the PKI file (MIGRPKIx).


| Variable | Default value | Definition |
| --- | --- | --- |
| OLDPKI |   | Transfer CFT PKI source file name |
| NEWPKI |   | Transfer CFT PKItarget file name. |


If the target version is lower than 3.4:

- Submit the procedure ..INSTALL(MIGRPKI) (export + import)

If the target version is 3.4 or higher:

- Submit the procedures ..INSTALL(MIGRPKI1) (export)
- Submit the procedures ..INSTALL(MIGRPKI2) (import)

1. Migrate the UCONF file parameters. (MIGRUCNF)

The JCL ..INSTALL(MIGRUCNF) must be customized to determine UCONF parameters to migrate.

Replace the line:

```
CFTEXT ID=
\*,TYPE
=UCONF,FOUT=$EXT
```

With the list of UCONF parameters to migrate. For example:

```
CFTEXT ID=cft.mvs.sginstal.\*,TYPE=UCONF,FOUT=$EXT
CFTEXT ID=cg.\*,TYPE=UCONF,FOUT=$EXT
CFTEXT ID=cft.multi_node.\*,TYPE=UCONF,FOUT=$EXT
CFTEXT ID=cft.cftcat.default_size,TYPE=UCONF,FOUT=$EXT
CFTEXT ID=cft.cftcom.default_size,TYPE=UCONF,FOUT=$EXT
CFTEXT ID=cft.cftlog.fname.atts,TYPE=UCONF,FOUT=$EXT
CFTEXT ID=cft.cftaccnt.fname.atts,TYPE=UCONF,FOUT=$EXT
Etc.
```

****Submit the procedure ..INSTALL(MIGRUCNF)****

1. Migrate the CATALOG file (MIGRCAT) for a non multi-node environment.

You can set the following variables in the MIGR$SET file and (or) in the PMIGR2 parameters:


| Variable | Default value | Definition |
| --- | --- | --- |
| OLDEXEC |   | Executable library of the Transfer CFT version to migrate. |
| OLDCAT |   | Transfer CFT CATALOG source file name. |
| NEWCAT |   | Transfer CFT CATALOG target file name |
| DISPCAT | ‘R’ | 'R' for replace, 'A' for ADD:<br/> ‘R’ = Create or recreate CFT CATALOG file<br/> ‘A’ = Will only create a Transfer CFT CATALOG file. |
| RECNBCAT | 50000 | Size in records if Catalog must be created. |
| TMPCAT | &amp;CFTENV..MCAT | Work file name. |
| TMPSCAT | 'CYL,(50,10)' | Size allocation for work file.<br/> Use 3 cylinders for every 1000 transfers to be migrated. |


****Submit the procedure ..INSTALL(MIGRCAT).****

1. Migrate the communication media file(s) (MIGRCOM)for a non mutli-node environment.

You can set the following variables in the MIGR$SET file or/and in the PMIGR2 parameters:


| Variable | Default value | Definition |
| --- | --- | --- |
| OLDEXEC |   | Executable library of the Transfer CFT version to migrate. |
| OLDCOM |   | Transfer CFT communication source file name. |
| NEWCOM |   | Transfer CFT communication target file name. |
| DISPCOM | ‘R’ | 'R' for replace, 'A' for ADD:<br/> ‘R’ = Create or recreate a Transfer CFT communication file.<br/> ‘A’ = Will only create a Transfer CFT communication file. |
| RECNBCOM | 5000 | Size in records if communication file must be created. |
| TMPCOM |   | Work file name. |
| TMPSCOM | 'CYL,(10,10)' | Size allocation for work file. |


****Submit the procedure ..INSTALL(MIGRCOM).****
