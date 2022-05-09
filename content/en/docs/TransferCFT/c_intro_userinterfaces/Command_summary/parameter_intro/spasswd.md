{
    "title": "spasswd",
    "linkTitle": "spasswd",
    "weight": "3280"
}### spasswd

#### CFTSEND/CFTRECV

****[ SPASSWD = *string* , _AUTH_]****

Password for the user who is sending the file. You can provide the SPASSWD directly or through an external file, and uses the following format:

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
