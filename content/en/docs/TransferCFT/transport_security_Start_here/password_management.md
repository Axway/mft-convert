---

    title: Password management (SPASSWD)
    linkTitle: Password management
    weight: 250

---
This section describes how to implement three different types of password management. For each of these methods, an example is provided that shows the server side configuration and an example user command from the client side. These management types are:

- [Static passwords](#Static)
- [External flat files](#External)
- [System level authentication](#System)

<span id="kanchor30"></span><span id="kanchor31"></span>

## About RPASSWD and SPASSWD

In addition to RUSER or SUSER, you can provide a password in the RPASSWD/SPASSWD fields to have user authentication, the same as users had if previously using FTP/SSH.

RPASSWD and SPASSWD can be provided directly as <span class="bold_in_para">****mypassw123****</span>, through an external flat file such as <span class="bold_in_para">****@fname****</span>,or using another system. Other system types include:

- Operating System User Management
- Transfer CFT UI User Access Base ([xfbadm](../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities#xfbadmusr1))
- Access Management System ([PassPort AM](../../internal_a_m_start_here/about_passport_am), [AM exit](../../internal_a_m_start_here/am_exits))

To use one of these other systems, set the <span class="code">`rpasswd/spasswd `</span>to the<span class="code">` keyword _AUTH_`</span> value and the <span class="code">`cft.server.authentication_method`</span> parameter to the appropriate authentication method. See also, <a href="../../admin_intro/uconf/uconf_directory" class="MCXref xref">UCONF parameters</a>.

<span id="Static"></span>

## Static passwords

### Sending a file to the server

#### Server: static configuration

```
CFTRECV
id=idf01,
ruser=username01,
rpasswd=password01
```

#### Client: user command

```
SEND part=server, idf=idf0, ruser=username01, rpasswd=password01
```

### Receiving a file from the server

#### Server: static configuration

```
CFTSEND
id=idf01,
imply=yes,
fname=file01,
suser=username01,
spasswd=password01
```

#### Client: user command  

```
RECV part=server, idf=idf01, suser=username01, spasswd=password01
```

<span id="External"></span>

## External flat files

The file containing the passwords must have the format:

```
partner01 username01 password01
partner01 username02 password02
\* username01 password03
\* \* password04
```

### Sending a file to the server

#### Server: static configuration

##### Unix

```
CFTRECV
id=idf01,
rpasswd=@passwfile
```

##### Windows

```
Windows
CFTRECV
id=idf01,
rpasswd=#passwfile
```

#### Client: user command

```
SEND part=server, idf=idf01, ruser=username01, rpasswd=password01
```

### Receiving a file from the server

#### Server: static configuration

##### Unix

```
CFTSEND
id=idf01,
imply=yes,
fname=file01,
spasswd=@passwfile
```

##### Windows

```
CFTSEND
id=idf01,
imply=yes,
fname=file01,
spasswd=#passwfile
```

#### Client: user command

```
RECV part=server, idf=idf01, suser=username01, spasswd=password01
```

<span id="System"></span>

## System level authentication

In addition to RPASSWD and SPASSWD you must specified the authentication method (uconf:cft.server.authentication\_method) to use.

The supported authentication methods are:


| Authentication method  | copilot.restapi.authentication_method  | Details  |
| --- | --- | --- |
| Operating System  | system  | The user/password is checked against the operating system.<br/> <blockquote> **Note**<br/> We strongly recommend that you set copilot.misc.createprocessasuser=yes when using the system option.<br/> </blockquote> **Unix**<br/> You must use <span ><code>cftsu </code></span>to create users as a superuser is required (sudo or root privilege) to create a group and assign a user to a group. Refer to <a href="" >Using system users - UNIX</a> for details.<br/> • Create a group "group1": <span >groupadd group1</span><br/> • Add user "user1" to group "group1": <span >usermod -a -G group1 user1</span><br/> **Windows**<br/> You require a superuser (administrative user account) to create a group and assign a user to a group.<br/> • Create a group "group1": <span >net localgroup group1 /add</span><br/> • Add user "user1" to group "group1": <span >net localgroup group1 user1 /add</span><br/> <blockquote> **Note**<br/> For a user belonging to a domain, use: domain\user1 instead of user1<br/> </blockquote>  |
| Access Management  | am  | This methods uses an indirection towards the Access Management system. The user/password is checked by the configured access management system: {{< TransferCFT/suitevariablesFlowManager  >}}, PassPort AM, or internal AM. |
| xfbadm database<br/> (UNIX and HP NonStop exclusively) | xfbadm  | The user/password is checked using the xfbadm base (see the <a href="../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities">xfbadmusr and xfbadmgrp utilities</a>).<br/> A user that can execute xfbadmusr/xfbadmgrp utilities can create users and groups after executing the <span ><code>profile </code></span>from the runtime directory.<br/> • Create a group "group1" with gid=200:<span > xfbadmgrp add -G group1 -p group1_pw -g 200</span><br/> • From the user prompt, to add a user "user1" to group "group1"enter: <span >xfbadmusr add -l user1 -p user1_pw -u AUTO -g 200</span> |


********<span class="autonumber"></span><span id="REST"></span>REST API server authentication method********

********<span class="autonumber"></span>
![](/Images/TransferCFT/authentication_copilot_server.png)********

> **Note**
>
> 1\. If copilot.restapi.authentication\_method = system, then your access management type must be set to either am.type= none, or both am.type=internal and am.internal.group\_database = system.

> **Note**
>
> 2\. If copilot.restapi.authentication\_method = xbfadm, then your access management type must be set to either am.type= none, or both am.type=internal and am.internal.group\_database = xbfadm.

### Sending a file to the server

#### Server: static configuration

```
CFTRECV
id=idf01,
rpasswd=_AUTH_
uconfset id=cft.server.authentication_method, value=system
```

#### Client: user command

```
SEND part=server, idf= idf01, ruser=username01, rpasswd=password01
```

In this case, username01/password01 is compared with what is defined in <span class="code">`uconf: cft.server.authentication_method`</span>.

### Receiving a file from the server

#### Server: static configuration

```
CFTSEND
id=idf01,
imply=yes,
fname=file01,
spasswd=_AUTH_
uconfset id=cft.server.authentication_method, value=system
```

#### Client: user command

```
RECV part=server, idf= idf01, suser=username01, spasswd=password01
```

In this case, username01/password01 is compared with what is defined in <span class="code">`uconf: cft.server.authentication_method`</span>.