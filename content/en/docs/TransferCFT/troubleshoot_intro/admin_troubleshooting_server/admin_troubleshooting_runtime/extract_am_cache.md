---

    title: Extract the Access Management Cache
    linkTitle: Extract the Access Management Cache
    weight: 360

---
If you need to check the {{< TransferCFT/axwayvariablesComponentLongName  >}} rights for a specific user, you can use the <span class="code">`EXTAMCACHE `</span>command to obtain this information.

****Command syntax: EXTAMCACHE****

## Parameters


| Parameter  | Description  |
| --- | --- |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/fname">FNAME</a>  | Selects the Access Management cache file. By default, this is the CFTAM file, which is located in the <span ><code>data </code></span>folder.  |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/fout">FOUT</a>  | Selects the output file. By default, it is the console.  |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/id">ID</a>  | Selects the user, or users, for which you want to obtain access rights. By default, the function returns all users.<br/> You can use the wildcard characters <span ><code>*</code></span> and <span ><code>?</code></span> to filter the user names. |


****Example****

The following command extracts the cache file information for all users and their rights to a text file called <span class="code">`AMuser.txt`</span>.

```
CFTUTIL EXTAMCACHE FOUT=AMuser.txt
```

Or, to filter on user names containing "admin":

```
CFTUTIL EXTAMCACHE ID=Admin\*, FOUT=export_cache
```

The following is an example output:

```
Access Management cache, last update at 2018-08-27T06:19:21.882+02:00
 
3 entries found in the Access Management Cache:
==> Application@Synchrony Has the following privileges:
CONFIGURATION:CFTPARM VIEW
TRANSFER                           RESUME
TRANSFER                           DELETE
TRANSFER                           CANCEL
TRANSFER                           SUBMIT
TRANSFER                           PAUSE
TRANSFER                           EXECUTE
TRANSFER                           VIEW
TRANSFER                           END
TRANSFER                           CREATE
CONFIGURATION:CFTRECV              VIEW
SERVICE:UI                         CONNECT
CONFIGURATION:CFTSEND              VIEW
FILTER:CATALOG                     DELETE
FILTER:CATALOG                     VIEW
FILTER:CATALOG                     EDIT
FILTER:CATALOG                     CREATE
CONFIGURATION:CFTCOM              VIEW
FILE                                VIEW
FILTER:LOG                          EDIT
FILTER:LOG                          DELETE
FILTER:LOG                          VIEW
FILTER:LOG                         CREATE
CONFIGURATION:CFTPART              VIEW
TRANSFER                           EDITFILE
TRANSFER                            EDIT
TRANSFER                            VIEWFILE
TRANSFER                            DELETEFILE
CONFIGURATION:CFTLOG                VIEW
CONFIGURATION:CFTDEST               VIEW
==> HelpDesk@Synchrony             Has no access
==> Partner@Synchrony                Has no access
```

 