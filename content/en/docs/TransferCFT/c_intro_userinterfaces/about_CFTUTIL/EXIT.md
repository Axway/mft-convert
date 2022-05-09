{
    "title": "EXIT - Quit CFTUTIL",
    "linkTitle": "EXIT - Quit CFTUTIL",
    "weight": "220"
}The command EXIT quits the CFTUTIL program.

Parameter descriptions &lt;/h2&gt;
----------------------------------

**[exitcode = n { 0 ...65535 }]**

By default there is an code that displays when you use the EXIT command to quit CFTUTIL. However, you can use the exitcode parameter to define a specific exit code, which replaces the default value.

****Example****

```
>CFTUTIL EXIT EXITCODE=3
 
EXIT EXITCODE=3
CFTU00I EXIT _ Correct (EXITCODE=3)
```
