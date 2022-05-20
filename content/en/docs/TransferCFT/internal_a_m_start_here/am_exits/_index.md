---
title: "About access management exits"
linkTitle: "Exit type access management"
weight: 170
---This section describes how to configure access management when not using {{< TransferCFT/PrimaryCGorUM  >}}.

{{< TransferCFT/axwayvariablesComponentShortName  >}} offers an access management exit in the form of a dynamic library. This library implements a set of mandatory functions described in the `$CFTINSTALLDIR/inc/exam.h` file.

Functions include:

- int exam_init(const char \*username)
- char\* exam_get_version(void)
- int exam_check_login(const char \*username, const char \*password)
- int exam_change_password(const char \*username, const char \*old_password, const char \*new_password)
- int exam_check_permissions(EXAMPermission \*\*perm_list)
- int exam_check_potential_permissions(EXAMPermission \*\*perm_list)

To help you get started, an Access Management exit sample is delivered in: `$CFTDIRRUNTIME/src/exit/cftexamsmp1.c`

For more information, see [Delivered Access Management exit samples](am_samples).

****Related topics****

[UCONf parameters](../../admin_intro/uconf/uconf_parameters)
