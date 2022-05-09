{
    "title": "Tracking a copied file ",
    "linkTitle": "Tracking a copied file",
    "weight": "210"
}This topic describes how you can copy a file to a different location without actually sending the file, yet still have the standard Transfer CFT tracking. This option allows you to copy a file, with visibility in either Transfer CFT or Central Governance, without using network resources.

Enable the copy file functionality
----------------------------------

This functionality is comprised of a delivered configuration and sample script files. To enable the copy file functionality, interpret the `cft-copyfile.conf` configuration file.

**Unix**

```
CFTUTIL @conf/cft-copyfile.conf
```

**Windows**

```
CFTUTIL \#conf\\cft-copyfile.conf
```

**OpenVMS**

```
CFTUTIL @CFT_SCEN:CFT-COPYFILE.CONF
```

**z/OS**

```
JCL ..INSTALL(CFTCOPYF)
```

**IBM i**

```
CFTUTIL PARAM('\#CFTPROD/CPYFILE(CPYFCONF)')
```

The configuration file contains two defined partners, COPY_SRC and COPY_DST, and a flow called COPYFILE. In the delivered sample configuration file both partners are local, but you can modify the HOST and SAP parameters, in the CFTTCP and CFTPART definitions respectively, to use two different {{< TransferCFT/axwayvariablesComponentLongName  >}} instances.

Define a request to copy a file
-------------------------------

Enter the following command to set up a send request to the COPY_DST partner:

```
CFTUTIL send part=COPY_DST, nfname='destination_file_name', parm='source_file_name'
```

Where:

- parm: The name of the file to be copied
- nfname: The name of the destination file

This creates an empty file that is transferred from COPY_SRC to COPY_DST. The empty file is then used to create the catalog record.

> **Note**
>
> Note: On a z/OS platform, use the following syntax and prefix the nfname with an asterisk ‘\*’.

```
send part = 'COPY_DST',
   parm = 'my.env.FTEST',
   nfname = '\*my.env.FTEST.&IDT'
```

How it works
------------

**UNIX/Windows**

On the sender side, an end-of-transfer script (exec/copyfile-snd.cmd) executes to update the information in the catalog, so that the fname parameter displays the parameter value used in the send command.

On the receiver side, an end-of-transfer script (exec/copyfile-rcv.cmd) executes to copy the file, write a log message, and update the catalog entries based on the copy results.

The same concepts apply for all OS, but there is a different syntax depending on the platform.

**OpenVMS**

- exec/copyfile-snd.pro
- exec/copyfile-rcv.pro

**z/OS**

- ..EXEC(COPYFILS)
- ..EXEC(COPYFILR)

**IBM i**

- CFTPROD/CPYFILE(CPYFSND)
- CFTPROD/CPYFILE(CPYFRCV)

### Copied file transfer details

When you view the transfer record for a copied file, either in the catalog or in {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, only certain transfer details display. For the actual copied file, only the FNAMEs, that is the source filename in the sent transfer and the destination filename on reception, are displayed. Other parameters related to the copied file, such as FLRECL, FRECS, FSPACE, and so on, are not available.

> **Note**
>
> Note: When using this functionality, the resulting catalog entry does not display the correct file size.

### OS specifics

The copy command used in the sample script is a system copy command that you can modify to better accommodate your needs. The commands are operating system dependent, as listed here:

- Unix: `cp`
- Windows and OpenVMS: `copy `
- IBM i and z/OS : `CFTUTIL/COPYFILE` command (valid for sequential or HFS files on z/OS systems)
