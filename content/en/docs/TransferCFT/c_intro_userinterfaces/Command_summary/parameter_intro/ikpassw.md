{
    "title": "ikpassw",
    "linkTitle": "ikpassw",
    "weight": "1620"
}<span id="ikpassw"></span>

### ikpassw

#### PKICER, PKIKEY

****[IKPASSW = { string64 &#124; filename } ]****

Used in PKIUTIL.

This is the source file protection password, and must be
specified for encrypted PEM (PKCS\#5), PKCS\#8 encrypted private key formats, PKCS\#7, or for PKCS\#12 certificate
formats (for PKICER only).

There are two ways to specify the password:

- By
    value: The value assigned to the parameter is used directly as a password.
- By
    reference to a file: The value assigned to the parameter is the name
    of a file, the first record of which contains the password; in this case,
    the file name must be preceded
    by a \# or @ sign depending on the OS. On Windows, for example, IKPASSW=\#myfile
    where the password is specified in the `myfile `file; the first file
    record must contain the password in plain format.

[Return to Command index](../../)
