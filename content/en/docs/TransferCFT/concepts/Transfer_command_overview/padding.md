---
    "title": "Pad records for text files",
    "linkTitle": "Padding records for text files",
    "weight": "330"
---
You can define the pad/unpad character for fixed and variable formats at both the file and network level in the SEND, CFTSEND, CFTRECV and RECV commands when TYPE = FILE.

<span id="Format"></span>

Format options
--------------

Use the [FRECFM](../../../c_intro_userinterfaces/command_summary/parameter_intro/frecfm) and [NRECFM](../../../c_intro_userinterfaces/command_summary/parameter_intro/nrecfm) parameters to set the record format.

### Fixed format

`FRECFM = F` or `NRECFM = F`

When using a fixed format, the parameters FPAD and NPAD define the character to use for padding. If FPAD and NPAD are not set, the character used for padding is a space.

### Variable format

`FRECFM = V` or `NRECFM = V`

When using a variable format, FPAD and NPAD define the character to use to unpad the record. If you do not set FPAD or NPAD, then the record remains unchanged and no unpadding occurs.

<span id="Paramete"></span>

Parameters
----------

`FPAD = character`

{{% TransferCFT/snippets/parm_fpad%}}

`NPAD = character`

{{% TransferCFT/snippets/parm_npad%}}

> **Note**
>
> Note: In addition to printable characters, you can also enter a non-printable character for NPAD or FPAD using the hexadecimal syntax: 0xHH. For example: 0xFC, 0xab, 0x00

Usage
-----

After the input or output file in each example a representation depicts the file sent or received over the network.

#### Example of padding a variable format file on the sender side

In the SEND profile (CFTSEND object) specify:

```
fcode=ASCII
frecfm=V
nrecfm=F
nlrecl=<desired_record_size>
npad=@
```

**Input file**

```
Axway
Transfer CFT
v 3.10
```

File sent over the network, when nlrecl=20:

```
Axway@@@@@@@@@@@@@@@
Transfer CFT@@@@@@@@
v3.0.1@@@@@@@@@@@@@@
```

#### Example of unpadding a fixed format file on the sender side

In SEND profile (CFTSEND object) specify:

Input file when flrecl=20:

```
Axway@@@@@@@@@@@@@@@
Transfer CFT@@@@@@@@
v 3.10
@@@@@@@@@@@@@@
```

File sent over the network:

```
Axway
Transfer CFT
v 3.10
```

#### Example of padding a variable format file on the receiver side

In the RECV profile (CFTRECV object) specify:

```
fcode=ASCII
frecfm=F
flrecl=<desired_record_size>
fpad=@
```

File received from the network:

```
Axway
Transfer CFT
v 3.10
```

**Output file**

```
Axway@@@@@@@@@@@@@@@
Transfer CFT@@@@@@@@
v 3.10
@@@@@@@@@@@@@@
```

#### Example of unpadding a fixed format file on the receiver side

In the RECV profile (CFTRECV object) specify:

``

fcode=ASCII

frecfm=V

fpad=@

File received from the network

```
Axway@@@@@@@@@@@@@@@
Transfer CFT@@@@@@@@
v 3.10
@@@@@@@@@@@@@@
```

Output file

```
Axway
Transfer CFT
v 3.10
```
