---

    title: Client authentication
    linkTitle: Client authentication 
    weight: 310

---
{{< TransferCFT/suitevariablesTransferCFTName  >}} REST API supports *HTTP Basic* and *HTTP Bearer* as the authentication method. Confidentiality is ensured by the use of an HTTPS connection.

We recommended that you use the HTTP Bearer as opposed to Basic method for the following reasons:

- You user/password is not exposed.
- If the token is compromised, you can revoke the token using either the UI (My Access Token) or REST API. Note though that a token is just as sensitive as a user/password, you must store it in a protected manner.
- If am.type=passport and the PassPort server is down, you can still execute REST API requests.

## Bearer authentication

To use this type of authentication you must specify the HTTP Authorization header using the following syntax:

`Authorization: Bearer <token>`

****Example****

```
curl -X GET "https://localhost:1768/cft/api/v1/transfers" -H "accept: application/json" -H "Authorization: Bearer eyJhbGnMQdLK6lwYPwy6-B3zrHYjhAqX5UUYht2zkd5-iSBbdyUYuVpSTMhA"
```

To use the bearer method, you require an access token as described below.

> **Note**
>
> REST API usage requires bearer authentication if SAML is enabled, am.type=saml.

### Generate an access token

In the {{< TransferCFT/axwayvariablesComponentLongName  >}} UI:

1. Navigate to your user login in the upper right hand corner.
1. Select **My Access Tokens** in the drop-down menu.  
    The **My Access Token** page displays.
1. Click **Generate Token**.  
    The Action, User, Creation date, and Token fields display. In the **Token** field, click the ![](/Images/TransferCFT/copy_icon.png)copy icon to easily copy the entire token.

## Basic authentication

To use this type of authentication you must specify the HTTP Authorization header using the following syntax:

`Authorization: Basic <base64(user:password)>`

****Example****

```
curl -X GET "https://localhost:1768/cft/api/v1/transfers" -H "accept: application/json" -H "Authorization: Basic Z3Vlc3Q6Z3Vlc3QK"
```

## Limit the number of failed login attempts

Transfer CFT provides brute force protection for logging on the {{< TransferCFT/suitevariablesTransferCFTName  >}} UI, REST API, or Web Services when using either the *system* mode or *xfbadm* mode (UNIX and HP NonStop only) authentication. That is, it limits the number of login failure attempts, where both the user and the password are checked to avoid brute force attacks.

For other authentication methods, such as PassPort and LDAP, no check is made. You must manage that in the Password Policy of those external tools.

You can use the following UCONF parameters to manage this option:

- `copilot.general.login_failures_fname`: A file that stores data shared between Transfer CFT and Copilot.
- `copilot.general.max_login_failures`: An integer that sets the maximum number of login failures for a user (default is 3, and 0 disables this option).

> **Note**
>
> In a multi-host environment, an attacker may have up to the copilot.general.max\_login\_failures \* &lt;number of host> tries before the user is locked if the file is not in a directory shared by all hosts.

When the maximum number of login failures is reached, the user account is locked for 30 seconds.

****Platform specifics****

- On IBM i systems, there is no action if the password is incorrect as the system offers methods that you can rely on to avoid brute force attacks (the system value is [QMAXSIGN](https://www.ibm.com/support/knowledgecenter/ssw_ibm_i_74/rzarl/rzarlmaxsgn.htm)).
- On z/OS systems, only the inherent system protection is available (refer to the RACF suboperand [REVOKE](https://www.ibm.com/support/knowledgecenter/SSLTBW_2.3.0/com.ibm.zos.v2r3.icha700/setrpw.htm) for the PASSWORD option).
