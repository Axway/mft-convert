{
    "title": "Customize the z/OS installation",
    "linkTitle": "Customize the z/OS installation",
    "weight": "230"
}Each target environment corresponds to a Transfer CFT. This topic describes the specific Transfer CFT customization. Transfer CFT customization is application dependent, as opposed to the target environment, which is system dependent.

JCL CFTENV (include member)
---------------------------

This member is customized during installation phase. It contains a list of JCL SET commands concerning the current instance, CFTENV must be included when PCFTUTIL and PCFTUTL procedures are used.

For z/OS 2.1, you can export JCL variables:

Export Global

Uncomment `//*     EXPORT SYMLIST=* in CFTENV` member

Or

Before INCLUDE MEMBER=CFTENV add directives

// EXPORT SYSMLIST=\*

// INCLUDE MEMBER=CFTENV

Example:

```
// LIB JCLLIB ORDER=(MY.CFTPROD.PROCLIB)

> // EXPORT SYSMLIST=\*
> // INCLUDE MEMBER=CFTENV
> //CFTSND EXEC PCFTUTIL,PARM='',QUAL=&CFTENV,OUT=&OUT
> //CFTPARM DD DUMMY to optimize
> //CFTIN DD \*,SYMBOLS=(CNVTSYS,SUBSLOG)
> SEND PART=PARIS,IDF=BIN,
> FNAME=&CFTENV..FTEST,
> IDA=’&SYSNAME-&ZOSLVL-&SYSCLONE’
> /\*

```

JCL CFTINC (include member)
---------------------------

You can use the CFTINC member in an INCLUDE statement to reference a list of Transfer CFT file allocation statements.

Example:

```
// LIB JCLLIB ORDER=(MY.CFTPROD.PROCLIB
//  INCLUDE MEMBER=CFTENV
//STREX EXEC PGM=IKJEFT01,REGION=32M,PARM='%REX4CFT'
//STEPLIB DD DISP=SHR,DSN=&CFTLOAD
//SYSPROC DD DISP=SHR,
//  DSN=MY.SYSPROC
//SYSTSPRT DD SYSOUT=&OUT
//SYSTSIN DD DUMMY
//    SET QUAL=&CFTENV
//     INCLUDE MEMBER=CFTINC
```

PCFTUTIL / PCFTUTL procedures
-----------------------------

It is recommended that you use only procedures to access the Transfer CFT utilities.

We provide two procedure examples:

- PCFTUTIL: All Transfer CFT files are allocated, except for the PKI database and LOG.
- PCFTUTL: Same procedure that PCFTUTIL, but CATALOG, COM, PART, PARM file are not allocated.

These procedures are customized during installation phase.

To run a CFTUTIL executable (default), for example:

```
//LIB JCLLIB ORDER=(MY.CFTPROD.PROCLIB)
//   INCLUDE MEMBER=CFTENV
//ABOUT EXEC PCFTUTL,PARM='ABOUT TYPE=CFT',
//   QUAL=&CFTENV
```

To run another CFTUTIL executable, for example CFTPKI:

```
//LIB JCLLIB ORDER=(MY.CFTPROD.PROCLIB)
//  INCLUDE MEMBER=CFTENV
//LISPKI EXEC PCFTUTL,PG=CFTPKI,PARM='',
//  QUAL=&CFTENV
//MYPKI DD DISP=SHR,DSN=&CFTENV..PKIFILE
//CFTIN DD \*
LISTPKI PKIFNAME = $MYPKI,CONTENT=FULL
/\*
```

### Recommendations to ease application migration

This section describes the migration of applications using Transfer CFT utilities.

It is recommended as far as possible not to point directly to the product libraries and work through procedures.

To do this, copy the following members into a specific PROCLIB:

- CFTENV
- CFTINC
- PCFTUTIL
- PCFTUTL

When changing the Transfer CFT version, you should get the new version of these members. A rollback is done by copying the backups of these members. This also applies to the end-of-transfer procedures.

<span id="JOB H80EXEC"></span>

Miscellaneous JCL templates
---------------------------

The target.EXEC library contains an example of Transfer CFT procedures:

- EXECSF: End of file send procedure
- EXECRF: End of file reception procedure
- SWIACC: Procedure submitted when SWITCHING the Transfer CFT accounting file
- SWILOG: Procedure submitted when switching the Transfer CFT log file
- EXECCRON: Procedure submitted by a CFTCRON command
- COPXLOG: Sample of JCL submitted by Transfer CFT Navigator (with dynamic parameter)
- COPEXT: Sample of JCL submitted by Transfer CFT Navigator to extract Transfer CFT parameter in a file
- CFTHEART: HEARTBEAT JCL for SENTINEL dashboards
- SNTLEXEC: End of file reception procedure including SENTINEL
- TFCIPH: Transfer CFT Preprocessing script for TrustedFile
- TFDCIPH: Transfer CFT End of transfer script for TrustedFile
- TFDELFIL: Transfer CFT Send exec script for TrustedFile
- CA7POST: Indicates to CA7 that the creation of a data set has completed
- COPYFILR and COPYFILS: templates that use the COPYFILE command
- CRONSTAR: Sample of the JCL that is submitted at Transfer CFT STARTUP
- CRONSHUT: Sample of the JCL that is submitted at Transfer CFT shutdown
- EXECIDF: End of file reception procedure sample with conditional steps
- EXECIFD2: The value of the IDF is checked among a list of values ​​by a REXX that sets a return code
- EXECIDF3: The JCL is conditional and uses the )SEL and )ENDSEL syntax - see [Syntax for )SEL and )ENDSEL](#Syntax)

These procedures are customized during the A00CUSTO phase.

<span id="Syntax"></span>

### Syntax for )SEL and )ENDSEL

#### )SEL argument1 op argument2

At least one space is mandatory between the arguments and operator.

The test is systematically performed on the length of the first argument, enabling generic arguments to be compared if necessary.

The maximum number of nested )SELs is 32, where:

- argument = constant, variable in the &xxx format
- op = condition operator

> = EQU or EQ: equal to

> &gt; or GT: greater than
>
> &lt; or LT: less than
>
> &gt;= or GTE: greater than or equal to
>
> &lt;= or LTE: less than or equal to
>
> &lt;&gt; or != or &#124;= or NEQ: different from

****Examples****

)SEL &P1 = SITE1: includes the following cards if parameter 1 is equal to SITE1

)SEL AG = &P1: includes the following cards if parameter 1 starts with AG

)SEL AG &lt;&gt; &P2: includes the following cards if parameter 2 does not start with AG

#### )ENDSEL

The )ENDSEL card does not contain any arguments.

The appropriate number of )ENDSELs required to terminate all active )ENDSELs are automatically added at the end of the JCL.

Update the unified configuration file CFT$SET
---------------------------------------------

The JCL CFT$SET, located in the target.INSTALL library, updates the unified configuration file and creates a Transfer CFT parameters sample (..SAMPLE(CFTPARM)) from a template (..SAMPLE(ZCFTPARM)).

The parameters in this JCL were customized during the (A00CUSTO) process, while customizing the CFTENV file (JCL variables).

List of updated variables:

- cft.runtime_dir
- cft.full_hostname
- cft.state_compat
- cft.listcat_compat
- cft.instance_id
- cft.instance_group
- samples.pesitany_sap.value
- samples.pesitssl_sap.value
- samples.coms_port.value

You can run the JCL multiple times, once to create the member .. SAMPLE (CFTPARM), which the procedure does not modify.

<span id="D40INIT"></span>

Format Transfer CFT work files <span id="kanchor43"></span>D40INIT
------------------------------------------------------------------

The JOB D40INIT prepares the Transfer CFT z/OS files. Before submitting this JOB, adapt the following points to the requirements of the operating service:

- File names (if the default values in the samples are not usable)
    -   By default file names are defined in JCL INCLUDE=CFTENV

<!-- -->

- The values of the parameters RECNB and FSPACE

The following data is required to use the basic Transfer CFT installation customization parameters. These work files are:

- CFTPARM: VSAM KSDS file, Transfer CFT parameters descriptions

<!-- -->

- CFTPART: VSAM KSDS file, Transfer CFT partners description

<!-- -->

- CFTCAT: VSAM ESDS file, Transfer CFT catalog

<!-- -->

- CFTLOG1: Sequential file used as the log file by Transfer CFT

<!-- -->

- CFTLOG2: Sequential file used by Transfer CFT as the alternate log file

<!-- -->

- CFTACNT1: Sequential file used as the account file by Transfer CFT

<!-- -->

- CFTACNT2:  Sequential file used by Transfer CFT as the alternate account file

<!-- -->

- CFTCOM: VSAM ESDS file used by Transfer CFT as a buffer for the Transfer CFT commands submitted by CFTUTIL, a BATCH program, a TSO user, the Transfer CFT Copilot, or the Transfer CFT UI

<span id="JOB E50PARM CFTPARM"></span>

CFTPARM configuration update <span id="kanchor44"></span>E50PARM
----------------------------------------------------------------

The JCL E50PARM, located in the target.INSTALL library, updates the Transfer CFT configuration PARAM and PART files (PARM step).

To activate the SFTP parameters:

1. -   In the CFTPARM MEMBER, uncomment the `/*   sftp,sftpcli, */` line.

1. -   Uncomment the following:
        -   `//* DD DISP=SHR,`
        -   `//* DSN=&QUAL..SAMPLE(CFTSFTP)`

```
E50PARM
//PARM EXEC PCFTUTIL,
// PARM='',
// QUAL=&CFTENV,OUT=&OUT
//CFTIN DD DISP=SHR,
// DSN=&QUAL..SAMPLE(CFTPARM)
//\*     DD DISP=SHR,
//\* DSN=&QUAL..SAMPLE(CFTSFTP)
The underlined parameters are substituted during the submit phase.
```

1. Use the JCL SAMPLE(PKIKEY) as an example for handling keys required for SFTP.
