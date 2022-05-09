{
    "title": "rpasswd",
    "linkTitle": "rpasswd",
    "weight": "3000"
}### rpasswd

#### CFTSEND/CFTRECV

****[ RPASSWD = *string*, _AUTH_]****

Password for the user who is receiving the file. You can provide this directly, or through an external flat file using the following format:

`part1 user1 passwd1`

`part2 user2 passwd2`

`* user1 passwd3`

`* * passwd4`

Or you can use _AUTH_ to indicate authentication method as defined in the uconf `cft.server.authentication_method` parameter.

****Example****

` /home/&USERID/cft_passw`

> **Note**
>
> Note: If you begin a password with an indirection character (Unix @, Windows \#), it is considered a reference to a file and not part of the password.

[Return to Command index](../../)
