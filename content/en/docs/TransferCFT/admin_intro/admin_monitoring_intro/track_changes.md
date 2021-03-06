---
title: "Tracking configuration changes"
linkTitle: "Tracking Transfer CFT configuration changes"
weight: 270
---The configuration auditing feature enables Transfer CFT to track configuration changes and send this information to the Sentinel
server. The configuration change can be:

* Deleting, modifying, or creating a CFTxxx object (PART, or PART database)
* Creating or deleting
    a CFTFILE object (PARM, PART, CAT, LOG…)

> **Note**
>
> The configuration change tracking feature described in this section is not available when using Central Governance or Flow Manager as Transfer CFT configuration changes are tracked by those products. Consequently, you cannot set the parameters described in this section using either of these products.

## Procedure

To enable configuration change auditing, set the following [unified configuration](../../uconf) (UCONF) parameters:

1. Activate the Sentinel connector by setting `sentinel.xfb.enable=yes`.
1. Activate the audit by setting `sentinel.xfb.audit=yes.`

**Example**

Using CFTUTIL, for example, define the parameters as follows:

```
CFTUTIL UCONFSET ID= sentinel.xfb.enable
, value=yes
CFTUTIL UCONFSET ID=sentinel.xfb.audit,
value=yes
```

## Message Track

Message Track is an XML XFBLog message
containing:

* Ident attribute
* Return message attribute
* Sentinel.xfb.audit

****Example****

```
/Action=CREATE /Object=CFTSEND /id=ZZ /user=My_Company\\giovanip /groupid= /owner= /CrDate=20191204 /CrTime=17471640 /UpdDate=20191204 /UpdTime=17471640
```
<span id="Ident attribute"></span>

### Ident attribute details


| CFTA0nX  | Details  |
| --- | --- |
| n=1 | CFTPARM file  |
| n=2 | CFTPART file  |
| n=3 | CFTCAT file  |
| n=4 | CFTCOM file  |
| n=5 | CFTLOG file  |
| n=6 | CFTACCNT file  |
| X=I  | Information  |
| X=E  | Error  |


<span id="Return message attribute"></span>

### Return message attribute details


| Attribute  | Details  |
| --- | --- |
| Action=&amp;act  |  &amp;act is the action on an object (“CREATE” , “MODIFY” , “DELETE”)  |
| Object=&amp;obj  | &amp;obj is the object identifier (type of object for the CFTFILE command)  |
| id=&amp;id  |   |
| user=&amp;user  |  &amp;user is the user that modified the object  |
| groupid=&amp;group  | &amp;group is the user group that modified the object  |
| owner=&amp;owner  | &amp;owner is the object owner  |
| CrDate=&amp;cdate  | &amp;cdate is the Creation Date for the object  |
| CrTime=&amp;ctime  | &amp;ctime is the Creation Time for the object  |
| UpdDate=&amp;upddate  | &amp;upddate is the Update Date for the object  |
| UpdTime=&amp;updtime  | &amp;updtime is the Update Time for the object  |

