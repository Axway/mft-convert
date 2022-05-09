{
    "title": "Transfer CFT communications",
    "linkTitle": "Transfer CFT communications",
    "weight": "320"
}This section describes the communication modes between users and {{< TransferCFT/axwayvariablesComponentShortName  >}}.

Using CFTUTIL
-------------

CFTUTIL, the command line interface for Transfer CFT, is able to translate Transfer CFT parameter setting commands and operating commands and can be activated in the following operational modes:

- Batch
- Command
- Interactive

### Batch mode

In batch mode, CFTUTIL executes the commands in a text file that are passed as a parameter. The file name must be preceded by either the \# or @ character.

Example:

`$ cftutil #cft_scen:cftdat.parm`

Or:

`$ cftutil @cft_scen:cftdat.parm`

In this example, CFTUTIL executes the commands in the `cft_scen:cftdat.parm` file.

```
CFTU20I
CFTU20I CFT/VMS
CFTU20I Version 3.10
04/04/2018
CFTU20I (C) Copyright AXWAY 1989-2018
CFTU20I ====> Starting Session on 04/04/2018 Time is 17:03:35
CFTU20I
CFTU00I CFTFILE  _ Correct (TYPE=PARAM,FNAME=CFTPARM)
CFTU00I CFTFILE  _ Correct (TYPE=PART,FNAME=CFTPART)
CFTU00I CFTFILE  _ Correct (TYPE=CAT,FNAME=CFTCATA,RECNB=600)
CFTU00I CFTFILE  _ Correct (TYPE=COM,FNAME=CFTCOM,RECNB=50)
CFTU00I CFTFILE  _ Correct (TYPE=LOG,FNAME=CFTLOG)
CFTU00I CFTFILE  _ Correct (TYPE=LOG,FNAME=CFTLOGA)
CFTU00I CFTFILE  _ Correct (TYPE=ACCNT,FNAME=CFTACCNT)
CFTU00I CFTFILE  _ Correct (TYPE=ACCNT,FNAME=CFTACCNTA)
CFTU00I RETURN   _ Correct (CODE=0)
CFTU20I Number of Command(s) 8
CFTU20I Number of error(s)   0
CFTU20I Ending   Session on 23/04/2018 Time is 17:03:45
CFTU20I Session active for  0:00:10
$
```

### Interactive mode

In Interactive mode, control is passed to the CFTUTIL utility, and the CFTUTIL&gt; prompts you to enter a command:

```
Enter a CFT command
$   cftutil
CFTU20I
CFTU20I CFT/VMS
CFTU20I Version 3.2.1 08/08/2015
CFTU20I (C) Copyright AXWAY 1989-2015
CFTU20I ====> Starting Session on 23/10/2015 Time is 17:08:03
CFTU20I
 
To exit CFTUTIL, press Ctrl+Z or enter /end
.
```

File-based communication
------------------------

File-based communication is the default communication mode. In the configuration file CFTCOM card, you must set the TYPE parameter to FILE and the FNAME parameter to CFTCOM for example. For more information, refer to the Transfer CFT 3.0.1 User's Guide or online help.

By default, CFTUTIL dialogs with {{< TransferCFT/axwayvariablesComponentShortName  >}} using the CFTCOM communication file. There are two ways to change the communication file:

- CFTUTIL CONFIG command

<!-- -->

- CFTCOM logical name

### CFTUTIL CONFIG command

Prior to submitting a command to {{< TransferCFT/axwayvariablesComponentShortName  >}}, you must change the default setting using the CONFIG command, by specifying the new file that you want to use for communication purposes.

```
$ cftutil
CFTU20I
CFTU20I CFT/VMS
CFTU20I Version 3.0.1 15/02/2012
CFTU20I (C) Copyright AXWAY 1989-2012
CFTU20I ====> Starting Session on 06/07/2012 Time is 14:58:27
CFTU20I
1:[CFU] config type=com,mediacom=file,fname=my_file
CFTU00I CONFIG   _ Correct (type=com,mediacom=file,fname=my_file)
2:[CFU]
```

- If you perform this operation, ****the configuration is lost**** when you exit the CFTUTIL utility. You must redo this operation each time that you use CFTUTIL.
- You cannot implement communications using another file in line command mode using this method.
- In batch mode, you must specify this command on the first line of the file so that subsequent commands are deposited in the new communication medium.

### CFTCOM logical name

Another way to change the communication file is to redefine the CFTCOM logical name so that it points to the required file, prior to using CFTUTIL. This is the preferred procedure. As long as the CFTCOM logical name points to the new file, CFTUTIL automatically uses it as the communication file.

```
$ define CFTCOM my_file
$ CFTUTIL
CFTU20I
CFTU20I CFT/VMS
CFTU20I Version 3.0.1 15/02/2012
CFTU20I (C) Copyright AXWAY 1989-2012
CFTU20I ====> Starting Session on 06/07/2012 Time is 15:01:04
CFTU20I
1:[CFU]
```

### COPILOT

You can use Copilot to configure the communication file by declaring the new communication media file in the relevant field of the ****Administration Files**** management tab in the Transfer CFT's ****Ongoing Configuration****.

### API

Your application can configure a new file name prior to submitting a command. For more information, refer to the Transfer CFT 3.0.1 User's Guide or online help.

In C for example:

```
rc = cftau("COM","F=my_file");
```

### Access rights

Communication file users must have read and write access rights for the file. If users belong to:

- The same group, file protection must be: S:RWED,O:RWED,G:RWE,W:

<!-- -->

- Different groups, file protection must be: S:RWED,O:RWED,G:RWE,W:RWE

You can also grant write access rights to authorized users only using ACLs on the file.

If users from various groups use the communication file, it is recommended that you set the CFT_LOCK logical name to SYSTEM and grant the SYSLCK privilege to the relevant users. For more information, see the [Transfer CFT parameter settings]() section.
