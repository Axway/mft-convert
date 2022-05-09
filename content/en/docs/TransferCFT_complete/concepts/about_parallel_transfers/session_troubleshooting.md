{
    "title": "Session related troubleshooting",
    "linkTitle": "Session related troubleshooting",
    "weight": "250"
}This section provides transfer examples that demonstrate parameter dependencies and typical log outputs, including errors, which you may encounter when using similar values. The goal is to understand the effect of parameter combinations, and be able to adapt settings to your particular needs.

- [Example: cft.server.max_session is less than MAXCNX](#Example:)
- [Example: cft.server.max_session is greater than MAXCNX](#Example:2)

See the [Frequently asked questions](../faq) for common questions and answers on parameter usage, license keys, and so on.

Remote (partner) diagnostic codes
---------------------------------

When troubleshooting, remember that if the [DIAG](../../../troubleshoot_intro/about_error_codes/about_diagnostic_codes/diagi_diagnostic_codes) is greater than 500, it refers to a remote issue. To find the actual DIAG, subtract 500 from the displayed code. If the DIAG is 962, for example, the issue is a remote problem corresponding to DIAG 462 (no data sent on network).

Examples
--------

<span id="Example:"></span>

### Example: cft.server.max_session is less than MAXCNX

The following example demonstrates using UCONF parameter cft.server.max_session=p, where:

- p = 0 (default)
- MAXCNX =s (limiting factor)

If p is any value other than zero, the smaller of the cft.server.max_session and MAXCNX values is used.

##### Scenario 1 - Requester configuration

When cft.server.max_session is less than MAXCNX on the requester side, MAXCNX is effectively equal to *p* (though there is no limitation on the server).

For example, if cft.server.max_session = 3, then MAXCNX is limited to 3 on the requester side.

**Requester output**

```
CFTT75E connect reject <IDTU=A00000FT PART=SUN35-5 IDF=T1 IDT=D2918351 416 MAXTRANS>
```

**Server output**

There is no output in this scenario.

**Scenario 2 - Server configuration**

When p (cft.server.max_session = 3 for example) is less than MAXCNX on the server side, it is determined by MAXCNX =p. (no limitation on requester side).

**Server output**

```
CFTH66E Incoming calls (1) rejected, ERROR=sessions (ctx) in use >= max_sessions (3&#124;3), PROTOCOL=?
```

**Requester output**

```
CFTT75E connect reject <IDTU=A000006O PART=WINZZ-5 IDF=T2 IDT=D2918471 302 R 0 2f2>
```
<span id="Example:2"></span>

### Example: cft.server.max_session is greater than MAXCNX

In this example, the MAXCNX value is the limiting factor.

##### Scenario 1 - Requester configuration

When cft.server.max_session is greater than MAXCNX on the requester side the determining value is MAXCNX, meaning that MAXCNX is the limiting value.

**Requester output**

```
CFTH09E Network connect request local error <PART=SUN35-4 NCR=416 NCS=MAXCNX NET=TCP>
```

**Server output**

There is no message / output on server side.

**Scenario 2 - Server configuration**

When p [max.session=64] is greater than s on the server side, it is determined by MAXCNX =s. So in our example, MAXCNX is set to 3.

**Server output**

When MAXCNX (3 partner transfers) is reached there is no server indication, only a message on the requester side.

There is no message / output on server side.

**Requester output**

```
CFTH11E Error Opening session <PART=WINZZ-4 EV=VNRELI ST=CN0022>
CFTT75E connect reject <IDTU=A0000074 PART=WINZZ-4 IDF=T4 IDT=D2919014 302 R 0 2f2>
```

Related topics

- [About parallel transfers](../)
- [FAQ and troubleshooting](../faq)
