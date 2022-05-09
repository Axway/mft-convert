{
    "title": "Use the TCP/IP network",
    "linkTitle": "Use the TCP/IP network",
    "weight": "290"
}To use the TCP/IP network, you must use the LOOPBACK option. From UCX 2.0 onwards, the default applied to this protocol is NOLOOPBACK.

Parameter settings
------------------

To implement TCP/IP with {{< TransferCFT/axwayvariablesComponentShortName  >}} from UCX 2.0 and higher, submit the following command from a privileged account.

```
$ ucx set protocol TCP/loopback
```

### Customization

The CFT$TCPWINDSZ logical name enables you to define the socket send quota and the socket receive quota to be used during the creations of transfer sockets.

This option requires you to set a system UIC, SYSPRV, BYPASS, or OPER privilege. Too low of a value slows the transfer speed, and too large of a value results in CFTTPRO obstruction, which in turn reduces the transfer speed.

### Using the programming interface

Objects in the {{< TransferCFT/axwayvariablesComponentShortName  >}} programming interface (API) are implemented using a shareable library. This facility is used to prevent the applications that are calling the programming interface functions, from having to be re-linked each time that {{< TransferCFT/axwayvariablesComponentShortName  >}} is updated.

Implementation
--------------

1. To implement a shared library, the operator responsible for installing applications must install the CFTAPISH.EXE image and generate the application.

<!-- -->

1. CFTAPISH.EXE must be installed as an image recognized by VMS. Available options are:

- The library is copied to the SYS$SHARE directory and the image is installed using a command, such as: $ install sys$share:cftapish.exe/share.  
      
    The application is linked by a link appli, appli.opt/opt type command, where appli designates the application object calling the {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs and appli.opt is an option file containing the following lines:

```
PSECT_ATTR = CFTAUM, NOSHR
sys$share:vaxcrtl.exe/share ! this option must not be
                            ! used on an AXP
                            ! system
sys$share:cftapish.exe/share .
```

- The library is located in a {{< TransferCFT/axwayvariablesComponentShortName  >}} -specific directory.

If the directory is cft_exe:, the image is installed using a command such as install cft_exe:cftapishr.exe/share. However, this image can later only be accessed using a logical name.  

**Example**

```
$ define CFTAPISHR cft_exe:cftapish.exe
```

The application is linked by a link appli, appli.opt/opt type command, where appli designates the application object calling the {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs and appli.opt is an option file containing the following lines:  

```
PSECT_ATTR = CFTAUM, NOSHR
sys$share:vaxcrtl.exe/share
CFTAPISHR/share
```

By default, the VMSINSTAL procedure installs the shareable image in the CFT_EXE: directory for the {{< TransferCFT/axwayvariablesComponentShortName  >}} user group.
