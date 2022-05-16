---

    title: spasswd
    linkTitle: spasswd
    weight: 3260

---
### spasswd

#### CFTSEND/CFTRECV

****\[ SPASSWD = *string* , \_AUTH\_\]****

Password for the user who is sending the file. You can provide the SPASSWD directly or through an external file, and uses the following format:

`part1 user1 passwd1`

`part2 user2 passwd2`

`* user1 passwd3`

`* * passwd4`

Or you can use \_AUTH\_ to indicate authentication method as defined in the uconf <span class="code">`cft.server.authentication_method`</span> parameter.

****Example****

` /home/&USERID/cft_passw`

> **Note**
>
> If you begin a password with an indirection character (Unix @, Windows #), it is considered a reference to a file and not part of the password.

[Return to Command index](../../)

 
