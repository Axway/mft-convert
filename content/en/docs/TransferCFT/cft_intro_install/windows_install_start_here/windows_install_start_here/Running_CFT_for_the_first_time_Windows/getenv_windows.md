---
    "title": "Define additional environment variables ",
    "linkTitle": "Define additional environment variables",
    "weight": "260"
---
When loading the Transfer CFT profile, files that are stored in the `profile.d` directory are also executed, and all defined environment variables are then available in the current environment. This enables you to use these variables in the Transfer CFT configuration or processing scripts.

When loading the profile, the files that are loaded depend on if you are using Batch or PowerShell:

- profile.bat: only .bat files are loaded
- profile.ps1: only .ps1 files are loaded

{{% TransferCFT/snippets/powershelllimitation%}}

How to define additional Transfer CFT environment variables
-----------------------------------------------------------

1. In the `%CFTDIRRUNTIME%/profile.d `directory, create a new file with `.bat` as the suffix. Add your customized variables in this file as follows. For example:  
    set MYVARIABLE01=TheVariableValue01  
    set MYVARIABLE02=TheVariableValue02
1. Execute the `profile `command.

See also the details on using [symbolic variables](../../../../../c_intro_userinterfaces/command_summary/symbolic_variables).
