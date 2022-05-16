---

    title: sortby
    linkTitle: sortby
    weight: 3220

---
### sortby

If you do not define this parameter, the order is the actual catalog record order.

#### DISPLAY

**\[SORTBY= { string list } \]**

Sorts the DISPLAY command information in an alphabetical or alphanumeric order, where the maximum length is 32 and the number of entries is limited to 10.

For example, to sort using a partner name and identifier, enter:

<span class="code">`CFTUTIL DISPLAY SORTBY=(PART,IDF)`</span>

Additionally, you can add a prefix to the sorting criteria to indicate the order of the returned content. Use the plus symbol (<span class="code">`+)`</span> for ascending or the minus symbol (<span class="code">`-`</span>) descending order, where the default value is +.

For example:

`CFTUTIL DISPLAY SORTBY=(-IDTU)`

You can use the <span class="code">`SORTBY `</span>option with any parameter that is used in the Transfer CFT catalog. You can view all catalog parameters in the Transfer CFT UI, or enter the following CFTUTIL command to display a complete list:

`CFTUTIL DISPLAY CONTENT=DEBUG `

Some useful parameters to sort by include <span class="code">`IDTU`</span> and <span class="code">`IDT`</span>. Or, to sort using a date and time indicator for example, enter:

`CFTUTIL DISPLAY SORTBY=(-DATEK,-TIMEK)`

#### LISTCAT

**\[SORTBY= { string list } \]**

Sorts the LISTCAT command information in an alphabetical/alphanumberic order, where the maximum length is 32, and the number of entries is limited to 10.

Records are immediately available after a catalog item is deleted, which may affect the order of the IDTU in the catalog. To view the catalog by IDTU, please see the [LISTCAT SORTBY](../../../about_cftutil/monitoring_cftutil_intro/listcat_command#sortby_example) example.

[Return to Command index](../../)
