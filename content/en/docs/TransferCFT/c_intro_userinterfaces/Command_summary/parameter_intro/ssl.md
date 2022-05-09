{
    "title": "SSL",
    "linkTitle": "ssl",
    "weight": "3350"
}<span id="SSL"></span>

### SSL

#### CFTPROT

****[SSL = identifier ]****

The identifier for the CFTSSL object that is used as security profiles.

In requester mode, this value is used as the default security profile.

In server mode, {{< TransferCFT/axwayvariablesComponentShortName  >}} refuses to start up if it does not find
the CFTSSL object that corresponds to the identifier specified here.

#### CFTPART

****[SSL = identifier ]****

Identifier for the CFTSSL command that
is used as security profiles in client mode or specifying additional checks
in server mode.

****About CFTPART and SSL****

The SSL parameter is used to associate a security profile with a partner definition.

For incoming calls, the server mode, the SSL parameter value must correspond to the identifier of a DIRECT=SERVER SSL command. In this case, the SSL command does not serve as a security profile, but is used to add additional partner-related controls.

For outgoing calls (client mode), the SSL parameter value must correspond to the identifier of a DIRECT=CLIENT SSL command. [FOR DETAILS see CFTPART]

****Server mode features****

Due to SSL protocol properties, the security profile in server mode must be known when the incoming network connection is established, prior to protocol-level identification of the partner. Therefore, the security profile in server mode is declared in the CFTPROT object.

If the partner has a specific security profile for the server mode, after the partner is identified (the partner network identity is compared with a CFTPART object), additional checks are performed between the profile and open SSL session elements:

- Authentication level check required (CFTSSL command USERCID and VERIFY parameters)
- Check of the security algorithms supported (CFTSSL command CIPHLIST parameter)
- Client certificate DN check (CFTSSL command DNUSER parameter)
- DN check of the authority that issued the client certificate (CFTSSL command DNISSUER parameter)

If a specific SSL profile exists (CFTSSL direct=SERVER), then there are additional checks such as for the DNUSER or DNISSUER. However, if the SSL profile does not exist, the warning message CFTT47W displays.

[Return to Command index](../../)
