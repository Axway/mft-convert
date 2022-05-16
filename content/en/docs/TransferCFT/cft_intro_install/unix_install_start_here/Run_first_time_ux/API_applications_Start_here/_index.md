---

    title: About  API applications
    linkTitle: Build API applications and exits
    weight: 190

---
This book describes one of two {{< TransferCFT/axwayvariablesComponentShortName  >}} programming interfaces,
the API applications. This interface enables {{< TransferCFT/axwayvariablesComponentShortName  >}} to work in conjunction
with external applications.

This book begins with this topic
which introduces the two application families that can be developed using
these two interfaces and the development kit contents for building APIs.

- Applications communicating
    with {{< TransferCFT/axwayvariablesComponentShortName >}} to submit and monitor transfers or query the catalog,
    for example. See [Using APIs.](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/Prog/API/Using_APIs.htm)
- Exits enabling
    user programs to take control during a send operation. See [Managing
    exits.](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/Prog/Exits/Managing_exits.htm)

{{< TransferCFT/axwayvariablesComponentShortName  >}} only provides a programming interface in C. This programming
interface can only be used if the development kit is installed on your
system, C compiler and associated tools.

This book is comprised of the following topics which describe how to
create an API application.


| Topic  | Details  |
| --- | --- |
| <a href="creating_an_api_application">Creating an API application</a> | Describes the procedure to create an API application in {{< TransferCFT/axwayvariablesComponentShortName  >}} UNIX. |
| <a href="creating_an_exit_file">Creating an exit file</a> | Describes how to create an exit file for {{< TransferCFT/axwayvariablesComponentShortName  >}} UNIX. |
| <a href="creating_a_directory_exit">Creating a directory exit</a> | Describes how to create a directory exit in {{< TransferCFT/axwayvariablesComponentShortName  >}} UNIX. |
| Creating an accounting exit | Describes how to create an accounting exit in UNIX. |


<span id="Development_kit_contents"></span>

## Development kit contents

The development kit used to integrate the {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs is divided
into several directories:

- *&lt;installdir>/lib/*
    containing all required libraries, in C, including:
- A *libcftapi.a*
    module: this library is required for any application using the Transfer
    CFT APIs
- A *libcftexa.a*
    module: this library is required for any application using the Transfer
    CFT directory exits
- A *libcftexf.a*
    module; this library is required for any application using the file EXITs
- A *libcftexe.a*
    module; this library is required for any application using the end of
    transfer EXITs

To generate a user application based on the {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs and use
the file exit function, you must link the following with the *libcftapi.a* and *libexe.a* libraries.

- &lt;installdir>/runtime/src/capi/ containing
    a command entry and catalog query example
- &lt;installdir>/runtime/src/exit/ containing
    simple examples of file exits, directory exits, and end
    of transfer exits
