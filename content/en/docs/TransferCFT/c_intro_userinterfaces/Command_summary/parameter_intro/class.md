{
    "title": "class",
    "linkTitle": "class",
    "weight": "400"
}<span id="class"></span>

### class

#### CFTNET

**[CLASS = n]  **{1..32}

Class of the local resource used to establish a connection with a partner. Class associated with this network resource.

> **Note**
>
> Note: You cannot define two CFTNETs that have the same CLASS value.

#### CFTTCP

**[CLASS = {1
&#124; n}]    ** {1 .. 32}

Logical class of the physical link. The maximum is operating system dependent
and equal to NM_MAX_CLASS.

Recommended maximum values are:

- Windows NT - 64
- UNIX - 8
- MVS - 32

[Return to Command index](../../)
