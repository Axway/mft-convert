---

    title: Define additional environment variables 
    linkTitle: Define additional environment variables
    weight: 260

---
When loading the Transfer CFT profile, files that are stored in the `profile.d` directory are also executed, and all defined environment variables are then available in the current environment. This enables you to use these variables in the Transfer CFT configuration or processing scripts.

When loading the profile, the files that are loaded depend on if you are using Batch or PowerShell:

- profile.bat: only .bat files are loaded
- profile.ps1: only .ps1 files are loaded

> **Note**
>
> When executing cft commands such as CFTUTIL or PKIUTIL in PowerShell, you must remove all spaces surrounding the comma (,). For example, instead of the command CFTUTIL send part=paris , idf=test enter CFTUTIL send part=paris,idf=test.

## How to define additional Transfer CFT environment variables

1. In the <span class="code">`%CFTDIRRUNTIME%/profile.d `</span>directory, create a new file with <span class="code">`.bat`</span> as the suffix. In this file, add your customized variables as follows. For example:  
    set MYVARIABLE01=TheVariableValue01  
    set MYVARIABLE02=TheVariableValue02
1. Execute the <span class="code">`profile `</span>command.

See also, [Windows-specific system functions](../../specific_system_functions).
