---

    title:  UCONF commands
    linkTitle: UCONF commands
    weight: 240

---
You can use the following commands to modify configuration values, check for defaults, or get more information about a parameter. For additional information on the unified configuration, see the section using the [unified configuration utility](../).

**UCONFSET**

Use to modify a technical parameter value.

```
UCONFSET id=PARAMETER-KEY,value=STRING
```

After modifying a uconf value, you typically must restart {{< TransferCFT/axwayvariablesComponentShortName  >}}. When the parameter flag is set to reconfig (=reconfig), you can use the reconfig command instead of a restart. You can check the parameter flag to see if reconfig is an option for that particular parameter.

Using spaces in a UCONFSET command value

When using CFTUTIL to define a UCONFSET value that contains spaces, the parameter value with spaces must be enclosed by single quotation marks that in turn are within double quotation marks. For example, to define a superuser with spaces use the following format.

```
CFTUTIL UCONFSET id=am.passport.superuser, value="'A B C D'"
```

**UCONFGET**

Use to
retrieve a single technical parameter value.

```
UCONFGET id=PARAMETER-KEY
```

Results: <span class="code">`PARAMETER-KEY=PARAMETER-VALUE`</span>

**UCONFUNSET**

Use to
return a specified parameter to the default value.

```
UCONFUNSET id=PARAMETER-KEY
```

> **Note**
>
> Certain uconf values are integrally linked to node type uconf values. If you modify these related values, and then delete the node value, the modified parameters remain in the configuration.

**LISTUCONF**

Use to display multiple technical parameter values.

```
LISTUCONF id=PARAMETER-KEY-PATTERN,scope=ALL|USER|DEFAULT,content=BRIEF|FULL|DEBUG
```

RECONFIG TYPE=UCONF

When TYPE=UCONF , the UCONF reconfigurable variables are reloaded. Note that only the UCONF parameters flagged with RECONFIG / IRECONFIG are affected.

> **Note**
>
> Tip  
> You can use the listuconf content=extract function with fout to extract the configuration with the passwords in clear text (normally passwords are hidden in uconf). Example: CFTUTIL LISTUCONF CONTENT=EXTRACT, FOUT=UCONF

****Get more information****

To get more information on uconf values, enter the command:

```
LISTUCONF id=PARAMETER,content=FULL
```

****Example****

```
LISTUCONF id=copilot.general.serverhost,content=FULL
```
