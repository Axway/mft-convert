---
title: "Usage: add a script for transfer errors"
linkTitle: "Usage: add a script for transfer errors"
weight: 190
---This topic describes how to add a script that will execute when send and receive errors occur. Use the EXECE command to set the script.For information on the processing phases and phase steps, see [Processing concepts](../../phase_and_phasestep).

## Step overview

1. Create your script, including variables to be replaced when the script is executed.
1. Use the EXECE command to point to the script.
1. Run a test error. The alias, for example &diagi, in the script is interpreted in real time and displays in the catalog.

### Parameter details

You can use EXECE to define the error file to execute when an error leads to the K (killed) phase step. This value overrides the CFTPARM EXECRE/EXECSE value.

****[EXECE { string max_length=512}]****

Error file to execute when an error leads to the K (killed) PHASESTEP. This value overrides the CFTPARM [EXECRE](../../../c_intro_userinterfaces/command_summary/parameter_intro/execre)/[EXECSE](../../../c_intro_userinterfaces/command_summary/parameter_intro/execse) value.

## Using the exece command

After you create the script to be executed when an error occurs, point to the script path.

- Use either `CFTUTIL send `or` CFTUTIL recv`
- To set the script to execute, enter: `exece=<yourscriptfileaddress>`
- You can set the default behavior in the configuration file using:
    -   Unix: `CFTUTIL @<conffile>`
    -   Windows: `CFTUTIL #<conffile> `
- Use the parameters `DIAGP `an `DIAGC `to specify a customized error that in the DIAGP/DIAGC catalog respectively.
- Use the command [CFTEXT](../../../c_intro_userinterfaces/about_cftutil/configuring_cft_start_here/cftext_command) to generate a configuration file with current parameters.
