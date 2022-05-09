{
    "title": "Using cfttell",
    "linkTitle": "CFTTELL - Retrieve system information",
    "weight": "260"
}UNIX and Windows only

This executable file retrieves system information, for example information needed to request a key. To use `cfttell`:

- Navigate to the` <CFTDIRINSTALL>/bin` directory
- Run cfttell

Options:

- -l, -L each bit of information is displayed on a new line, this is the default option
- -s, -S values are listed on a single line, separated by spaces using the format key=value, this option is valid for target, version, and uid.
- -h display this help

Keys:

- TARGET: lists the target platform
- VERSION: returns the Transfer CFT version if Transfer CFT is installed
- HOSTINFO: lists the OS, Transfer CFT version, and related host and machine details
- UID: Generates a unique id.
- HOSTINFO: Display information used in key generation.

****Examples****

```
C:\\projects> cfttell target
win-x86-32
```

 

```
C:\\projects> cfttell version
3000
```

 

```
C:\\projects> cfttell hostinfo
CFT version : 3010
Target : win-x86-32
Processor architecture : x64
Processor type : Intel/AMD X8664
Processor ID :
Number of processors : 2
OS release : Windows Seven Service Pack 1
OS version : 6.1.7601
Host name : ITEM-12345
```
