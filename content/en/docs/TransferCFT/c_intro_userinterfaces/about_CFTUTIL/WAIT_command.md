{
    "title": " Suspend or quit CFTUTIL",
    "linkTitle": "WAIT - Suspend CFTUTIL",
    "weight": "210"
}WAIT
----

<span id="About_the_WAIT_Command"></span>Use this command to suspend the execution of CFTUTIL for
the time indicated.

****Command syntax: [WAIT](../../command_summary#WAIT)****

**Parameters**

**[DURING = {<span class="underline">0</span> &#124; n}]**

{0..65535}

Time in seconds that CFTUTIL is deactivated.

EXIT
----

The command EXIT quits the CFTUTIL program.

****Command syntax: EXIT****

**Parameters**

**[exitcode = {<span class="underline">0</span> &#124; n}]**

By default there is a code that displays when you use the EXIT command to quit CFTUTIL. However, you can use the exitcode parameter to define a specific exit code, which replaces the default value.

****Example****

```
>CFTUTIL EXIT EXITCODE=3
 
EXIT EXITCODE=3
CFTU00I EXIT _ Correct (EXITCODE=3)
```
