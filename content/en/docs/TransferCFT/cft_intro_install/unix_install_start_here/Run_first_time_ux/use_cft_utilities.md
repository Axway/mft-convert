{
    "title": "Management utilities",
    "linkTitle": "Management utilities",
    "weight": "190"
}This section describes Transfer
CFT UNIX utilities located in the `cft/<installdir>/bin/`
sub-directory after completing installation.

> **Note**
>
> Note: In this section, the term
> Transfer CFT designates the Transfer
> CFT software package on UNIX platforms.

The utilities described here, do not replace the
basic commands described elsewhere in this document. Their purpose is
to simplify common tasks performed with {{< TransferCFT/axwayvariablesComponentShortName  >}}.

Utility descriptions
--------------------

The following utilities are detailed in this page.

<table>
   <thead>
      <tr>
         <th><p>Utility  </p>         </th>
         <th><p>Definition  </p>         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>[cftinit](#cftinit)         </td>
         <td>General Transfer CFT initialization utility.         </td>
      </tr>
      <tr>
         <td>[cftutil](#cftutil)         </td>
         <td>Simplified display of the standard CFTUTIL commands.         </td>
      </tr>
      <tr>
         <td>[cftupdate](#cftupdate)         </td>
         <td>Management Utility updating the Transfer CFT configuration.         </td>
      </tr>
      <tr>
         <td><p>[cftcatal](#cftcatal)  </p>         </td>
         <td><p>Utility migrating and/or extending the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog file.  </p>         </td>
      </tr>
      <tr>
         <td><p>[xfbadmgrp](#xfbadm)  </p>         </td>
         <td><p>Group management utility (all users accessing the Transfer CFT Copilot server).  </p>         </td>
      </tr>
      <tr>
         <td><p>[xfbadmusr](#xfbadmusr1)  </p>         </td>
         <td><p>Utility managing users accessing the Transfer CFT Copilot server.  </p>         </td>
      </tr>
      <tr>
         <td><p>[xvi](#xvi)  </p>         </td>
         <td><p>Utility processing the conversion tables.  </p>         </td>
      </tr>
      <tr>
         <td><p>[atoe](#Conversion_tables)  </p>         </td>
         <td><p>ISO 8859-1 ASCII to EBCDIC conversion table.  </p>         </td>
      </tr>
      <tr>
         <td><p>[etoa](#Conversion_tables)  </p>         </td>
         <td><p>EBCDIC to ISO 8859-1 ASCII conversion table.  </p>         </td>
      </tr>
   </tbody>
</table>

<span id="cftinit"></span>

cftinit
-------

*cftinit* is a general {{< TransferCFT/axwayvariablesComponentShortName  >}}
initialization utility.

**Syntax**

`cftinit [<filename> [<filename>...]]`

**Standard use**

*cftinit* is normally used with a single
parameter, which is the name of the {{< TransferCFT/axwayvariablesComponentShortName  >}} configuration file.

`cftinit my_config.cft`

**Advanced use**

You can include several file names in the command line. Normally, all
{{< TransferCFT/axwayvariablesComponentShortName  >}} parameters are declared in a single file. However, for organizational
reasons, you may wish to separate the configuration into several files
(for example, a file describing the CFTPART cards and another file containing
the CFTPARM, CFTLOG cards, and so on).

`cftinit partners.cft the_rest.cft`

> **Note**

- If no file name
    is passed as a parameter, the program requests one or more file names.
- If no name is supplied,
    the program stops.
- When you run `cftinit`, it creates the catalog and communication files. You can modify the default sizes of these files to suit your requirements by updating the uconf values for `cft.cftcat.default_size` and `cft.cftcom.default_size` (these values are expressed as a number of records).

<span id="cftupdate"></span>

cftupdate
---------

The *cftupdate* utility is used to update the configuration.

**Syntax**

`cftupdate <filename> [<filename> ...]`

> **Note**

- You can only update
    the CFTPART, CFTxxx (for the networks), CFTSEND cards, and so on
- This command should
    be considered to be an alias of CFTUTIL @&lt;filename&gt; for each file
    name passed as a parameter in the command line

<span id="cftutil"></span>

cftutil
-------

The *cftutil* command submits a standard CFTUTIL instruction, but
displays the results without a banner. In addition, if the command return
code is non-null, a message is displayed.

**Syntax**

`cftutil <command>`

**Use**

```
% cftutil listcat type=z
CFTU26E LISTCAT _ Error (TYPE Bad value for parameter)
cftutil code 115
%
```
<span id="cftcatal"></span>

cftcatal
--------

You can use the *cftcatal* utility to increase
the size of the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog file without losing information. In a multi-node environment, this action resizes all nodes.

**Syntax**

`cftcatal`

<span id="xfbadm"></span>

xfbadmgrp
---------

The *xfbadmgrp* utility is used
to create, delete, modify and check a group (of users) with access rights to
the Transfer CFT Copilot server. It can be used in interactive mode associated with a command
(add, delete, and so on) or in batch mode, specifying each of the required
commands (-G group –p passwd, and so on).

****Syntax****

Add a user group:

`xfbadmgrp add [-G <group>] [-p <passwd>] [-g   <GID>] [-u <users>]`

Delete a user group:

`xfbadmgrp delete [-G <group>]`

Modify a user group:

`xfbadmgrp modify [-G <group>] [-p <passwd>] [-g   <GID>] [-u <users>]`

Display information on existing groups:

`xfbadmgrp print [-G <group>]`

This command displays information on a given group (if the -G option
is used) or on all existing groups.

****Standard use****

`xfbadmgrp  add   &#124; delete &#124; modify &#124; print &#124; check &#124; help`

****Advanced use****

Various options can be used to make it easier to enter information or
allow you to work in batch mode:

- -G &lt;group&gt;: ASCII name of the user group
- -p &lt;passwd&gt;: Password required to access this group
- -g &lt;GID&gt;: Numeric identifier of the group. If it is set to AUTO, the GID is generated
    automatically
- -u &lt;usr1,usr2&gt;: List of existing users, separated by a comma

<span id="xfbadmusr1"></span>

xfbadmusr
---------

You can use the `xfbadmusr`
utility to create, delete, check, and modify a user with access rights
to the Transfer CFT Copilot server. It can be used in interactive mode associated with
a command (add, delete, and so on) or in batch mode, specifying each of
the required commands (-G group -p passwd, and so on).

****Syntax****

Add a user. If the group does not exist, it is automatically created with the user
login name.

`xfbadmusr add [-l <login>] [-p <passwd>] [-u   <UID>] [-g <GID>]`

Delete a user. Users in the `group `file are automatically deleted from all the groups
with which they are associated.

`xfbadmusr delete [-l <login>]`

Modify a user. If necessary, modifications are applied automatically to the `group `file.

`xfbadmusr modify [-l <login>] [-p <passwd>] [-u   <UID>] [-g <GID>]`

Check a user.

`xfbadmusr check [-l <login] [-p passwd]`

Display information on existing users. Display information on a given user (if the -l option
is used) or on all existing users.

`xfbadmusr print [-l <login>]: `

****Standard use****

`xfbadmusr add &#124; delete &#124; modify &#124; print &#124; check &#124; help`

****Advanced use****

You can use the following options to make it easier to enter information,
or to work in batch mode:

- ****-l
    &lt; login &gt;****: Login name
- ****-p
    &lt; passwd &gt;****: Password
- ****-u
    &lt; UID &gt;****: User identifier - When set to AUTO, a UID is generated
    automatically
- ****-g
    &lt; GID &gt;****: Group identifier - When set to AUTO, the GID is
    generated automatically

****Example****

```
xfbadmusr add -l user1 -p thepassword -u AUTO -g AUTO
```

To check that the user1 is created, run:

```
xfbadmusr print
```

The output should resemble the following:

```
user1:$6$2clPU2CY..2clPU2$g0cm8rHz8X0Fvu1lz7TUVa2YfpPMkbs03wQWhd5f0IMEWDbCQHK9IumSObNF4voLEM/BlsSdNMlw1k01iPOdv0:106:106:::
```
<span id="xvi"></span>

xvi
---

The *xvi* utility is used to update a conversion table.

**Syntax**

`xvi [-d &#124; -a &#124; -e &#124; -l   <file> ] <table>`

****Standard use****

`xvi <table>: updates an existing, valid <table> (256 characters).`

**Advanced use**

The following options can be used with *xvi*:

- -d: displays an existing, valid &lt;table&gt;
    in ASCII
- -a: creates a &lt;table&gt; to convert
    ASCII to EBCDIC; this table is identical to the one accessed via the Transfer
    CFT CFTXLATE command (if &lt;table&gt; exists, it is overwritten)
- -e: creates a &lt;table&gt; to convert
    EBCDIC to ASCII; this table is identical to the one accessed via the Transfer
    CFT CFTXLATE command (if &lt;table&gt; exists, it is overwritten)
- -l:
    creates a &lt;table&gt; from an ASCII &lt;file&gt;; the file generally
    used is the file produced after running option -d (if &lt;table&gt; exists,
    it is overwritten)

<span id="Conversion_tables"></span>

Conversion tables
-----------------

By default, {{< TransferCFT/axwayvariablesComponentShortName  >}} uses internal tables to convert ASCII characters
to EBCDIC and vice versa. They are based on the ASCII character set as
defined on PC/DOS systems.

To perform a conversion using the ISO 8859-1
ASCII character set, run the CFTXLATE command with the
following external conversion tables:

- atoe:
    ISO 8859-1 ASCII to EBCDIC
- etoa:
    EBCDIC to ISO 8859-1 ASCII

You can use the [xvi](#xvi) utility, described above,
to create specific conversion tables or modify existing tables.
