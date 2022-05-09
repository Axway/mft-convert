{
    "title": "dnuser",
    "linkTitle": "dnuser",
    "weight": "760"
}<span id="dnuser"></span>

### dnuser

#### CFTSSL

****dnuser='(string1, string2)'****

****{{< TransferCFT/axwayvariablesComponentShortName  >}} syntax****

The new syntax is listed here. For continued compatibility, you can still use
the 2.4.x syntax.

-  
    dnuser=’ (“string1“ Op “string2“)’  
    Where the remote certificate DN must contain string1 Op string2, and Op
    is a OR/OU binary operator or AND/ET binary operator
-  dnuser=’ ( “string1“ Op ! “string2“)’   
    Where the remote certificate DN must contain string1 Op NOT(string2), and
    Op is a  OR/OU binary operator or AND/ET binary operator.

> **Note**
>
> Note: The different attributes of the dnuser or dnissuer string
> are separated by the '/' character.

[Return to Command index](../../)
