{
    "title": "Submitting  a transfer request",
    "linkTitle": "Submitting a transfer request",
    "weight": "180"
}After configuring the OFTP (ODETTE) environment, as described in [Configuring OFTP](../configuring_odette), you must
define the transfer environment in the following objects:

- CFTSEND
- CFTRECV

This topic describes the steps that enable you to
submit a transfer request. The [next
topic](../receiving_transfers) describes the procedure that enables you to receive. The parameters
for the CFTSEND object in ODETTE are defined in the following sections:

- [Data
    code](#Data_code)
- [Setting
    the type parameter](#Setting_the_type_parameter)
- [Setting
    the file format](#Setting_the_file_format)

<span id="About_CFTSEND_in_Odette"></span>

About CFTSEND
-------------

<span id="Data_code"></span>

### Data code

Regardless of the protocol format conveyed, data code information is
not sent. In reception mode, the Transfer CFT monitor considers that the
data code received is ASCII.

To ensure that alphanumerical data are sent in ASCII you can either:

- Set SYST = UNIX
    in CFTPART.  
    As the partner is defined as an ASCII system, data are then sent in
    ASCII (NCODE not defined) by default. Or,
- Set NCODE = ASCII
    in CFTSEND

Translation is carried out during transmission, if you set:

- FCODE = EBCDIC
- FCODE = ASCII and
    an external ASCII/ASCII translation table is defined

There is no translation if FCODE = BINARY.

The default value of FCODE, which is determined dynamically for each
transfer, depends on the file type, [FTYPE](../../../c_intro_userinterfaces/command_summary/parameter_intro/ftype),
sent. The automatic detection of FTYPE by the sender, as the case may
be, partly takes the place of automatic detection of FCODE.

<span id="Setting_the_type_parameter"></span>

### Setting the type parameter

You must set the NTYPE parameter to T, NTYPE = T, regardless of the
operating system.

Do not
set the following parameters:

- NRECFM: the format
    of a file is imposed for Odette text files
- NLRECL: the maximum
    length of records is set to 2048 bytes

The corresponding values are implicitly sent by Transfer CFT.

<span id="Setting_the_file_format"></span>

### Setting the file format

Set the RECFM parameter to:

- F for a file in
    fixed format
- V for a file in
    variable format
- U for a file in
    undefined format

> **Note**
>
> Note: Do not set the NTYPE parameter.

For fixed and variable formats, set the NLRECL parameter to indicate
the maximum length of records, excluding delimiters if any.

For
the undefined format, do not set the NLRECL parameter. It is
implicitly set to 0 and conveyed by the protocol.

The default values of the NRECFM and NLRECL network parameters are the
same as the values of the FRECFM and FLRECL local parameters. In most systems, the monitor is able to locate the corresponding
real values when the associated network parameters and local parameters
are not defined.
