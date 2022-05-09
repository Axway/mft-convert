{
    "title": "System requirements",
    "linkTitle": "System requirements",
    "weight": "150"
}The following are the system requirements for {{< TransferCFT/axwayvariablesComponentShortName  >}}.

About the {{< TransferCFT/axwayvariablesComponentShortName  >}} environment
--------------------------------------------------------------------------------

### Unix/Linux system prerequisites

Transfer CFT requires that you install the appropriate library for your operating system:

- Linux requires the ncurses version 5 library
- Unix systems, except Linux, require the curses library

### Supported operating systems and browsers

Refer to the {{< TransferCFT/axwayvariablesPlatformorSuiteLongName  >}} Supported Platforms guide available on {{< TransferCFT/axwayvariablesCompanyName  >}} Support at [https://support.axway.com](https://support.axway.com/).

Disk space and RAM requirements
-------------------------------

{{< TransferCFT/axwayvariablesComponentLongName  >}} has the following hardware requirements:

- Disk space requirement
    -   1.5 to 5 Gigabyte: minimum disk space to allow for future updates, SPs, and continued performance
- RAM Requirement
    -   128 Megabyte: minimum dedicated per host

Java
----

If you intend to implement Secure Relay you require Java 8, which is not delivered with Transfer CFT (except on HP PARISC and WIN IA64 where only Java 6 is delivered).

When using Secure Relay, Java must be installed in the same environment as the Transfer CFT installation. The Master Agent is managed, but the Router Agent can be in another environment.
