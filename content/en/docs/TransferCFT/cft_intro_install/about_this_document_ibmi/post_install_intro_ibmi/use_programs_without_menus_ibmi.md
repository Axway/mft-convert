---

    title: How to use programs without menus
    linkTitle: How to use programs without menus
    weight: 210

---
This section describes ho to use programs such as CFTUTIL, CFTINIT, CFTMI, etc. For example, to migrate Transfer CFT you cannot use the standard Transfer CFT IBM i menus and require the following steps.

1. Enter: <span class="code">`CALL `</span>and press <span class="bold_in_para">****F4.****</span>
1. In the <span class="bold_in_para">****Program**** </span>field, enter the name of binary (<span class="code">`CFTUTIL `</span>in the example).
1. In the <span class="bold_in_para">****Library**** </span>field, enter your program library (<span class="code">`*LIBL`</span>by default, <span class="code">`CFTPROD `</span> in the example).
1. In the <span class="bold_in_para">****Parameters**** </span>field you can enter an argument. CFTUTIL requires at least one argument (<span class="code">`CFTEXT `</span>in the example).

 
