---

    title: Password authentication 
    linkTitle: Password authentication: synchronous communication
    weight: 240

---
## About password authentication for synchronous communication media

Password authentication lets you control remote users that perform CFTUTIL commands via a synchronous communication media.

<span id="kanchor26"></span>

## Enabling password authentication on a Transfer CFT server

To enable password authentication set the authentication feature to yes, and specify the authentication method using these unified configuration parameters:

- `cft.server.cftcoms.authentication_enable`
- `cft.server.authentication_method`

Available authentication methods are:

- Operating System: value=system
    -   The user/password is checked against the Operating System values. For Unix environments, you must enable <span class="code">`cftsu `</span>as described in <a href="#Enable2" class="MCXref xref">How to use system user authentication for the user interfaces</a>
- Access Management: value=am
    -   The user/password is checked by the configured access management system (either PassPort AM, or the AM exit)
- Transfer CFT UI User Access Base (UNIX and HP NonStop only): value=xfbadm
    -   The user/password is checked using the xfbadm base
    -   Refer to [xfbadmusr and xfbadmgrp utilities](../../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities) for more information

See Related topics below for links to more information on these access management types.

## Configuring the communication media

1. Define the unified configuration settings so that you enable authentication yes, and select the method.


| Parameters  | Default  | Description  |
| --- | --- | --- |
| cft.server.cftcoms.authentication_enable  | No  | Authentication for synchronous communication:<br/> • Yes: Enable password authentication<br/> • No: Disable authentication |
| cft.server.authentication_method  | None  | Authentication method can be:<br/> • none: No method defined<br/> • system: Operating system<br/> • am: PassPort AM or AM exit<br/> • xfbadm: <a href="../../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities#xfbadmusr1">xfbadmusr</a> utility |


1. Define the following parameters.

```
CONFIG TYPE=COM, MEDIACOM=TCPIP, FNAME=protocol://host:port, PASSWORD=password
```

The username/password is then checked for each subsequent request. The username used for the authentication is the user that is currently logged in. api sample

****Related topics****

- [Command summary](../../command_summary)
- [PassPort AM](../../../internal_a_m_start_here/about_passport_am)
- [Access Management exits](../../../internal_a_m_start_here/am_exits)
- [CONFIG - Setting default CFTUTIL file names](../redefining_cftutil_data_media)
