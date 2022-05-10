---
    "title": "Use processing scripts",
    "linkTitle": "Standard use of processing commands",
    "weight": "170"
---
There are 4 stages where you can configure processing - preprocessing, post-processing, ack processing, and for a transfer error. For these, Transfer CFT provides global definitions in the static configuration that are used by default. However, you can override the default scripts, in the CFTSEND, SEND, CFTRECV and RECV commands, using the following parameters:

- `Preexec`: for preprocessing
- `Exec`: for post-processing
- `Ackexec`: for acknowledge processing
- `Exece`: for transfer errors

> **Note**
>
> Note: The exceptions to these rules are described in the following sections.

Methods for executing processing scripts
----------------------------------------

There are two ways to execute a processing script, either by referencing a template processing script, or by directly executing a program or a processing script. In either method, you can use symbolic variables, though they are processed differently as described below.

### Executing a template processing script

Using this method, {{< TransferCFT/axwayvariablesComponentLongName  >}} creates a temporary file based on the template processing script, and replaces all symbolic variables with the corresponding values as they relate to the transfer. For example, if &IDTU is in the script, it is replaced by the actual transfer value. {{< TransferCFT/axwayvariablesComponentLongName  >}} then executes this temporary file.

For example, to run the `myscript.sh` script using this method:

```
cftsend id=flow01, exec='exec/myscript.sh'
```

****Example of a template processing script****

```
CFTUTIL WLOG MSG="execute processing script for the &IDTU transfer "
CFTUTIL END PART=&PART, IDTU=&IDTU
```

****Example of the corresponding temporary file to execute****

```
CFTUTIL WLOG MSG="execute processing script for the A0000001 transfer "
CFTUTIL END PART=PART01, IDTU=A0000001
```

****Operating system differences****

Depending on the operating system, the temporary file is treated as follows:

- Windows: The temporary file is automatically deleted.
    -   To keep temporary files, you can set CFTNODEL as described in [Transfer scripts and temporary files](../../../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows/transfer_procedures_win).
- z/OS: Transfer CFT writes directly to a JES internal reader.
- IBM i: The temporary file is automatically deleted. [voir RDS]
- OpenVMS:The temporary file is automatically deleted. [voir RDS]
- UNIX: You must add the following lines at the end of the template processing script:

    `rm $0 `

    `rm $0.err`

    -   See [Transfer scripts and temporary files](../../../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/cft_temporary_files)for more information.

- HP NonStop

    -    native environment: You must perform the following steps to remove the temporary file:
        &lt;ul&gt;&lt;li&gt;\#PURGE [\#IN]&lt;/li&gt;&lt;li&gt;The same BTPURGE procedure as in the previous version is delivered and can be executed&lt;/li&gt;&lt;span class="code"&gt;&lt;code&gt;RUN &lt;subvolume&gt;UP.BTPURGE [\#DEFAULTS]&lt;/code&gt;&lt;/span&gt;&lt;/ul&gt;&lt;/li&gt;&lt;li&gt;OSS environment: You must add the following lines at the end of the template processing script: &lt;p class="code"&gt;&lt;code&gt;rm $0 &lt;/code&gt;&lt;/p&gt;&lt;p class="code"&gt;&lt;code&gt;rm $0.err&lt;/code&gt;&lt;/p&gt;&lt;/li&gt;&lt;/ul&gt;&lt;/li&gt;

    <span id="Directly"></span>

    ### Directly executing a program or a processing script

    **Available on Windows and Unix**

    A second method for executing scripts is to directly run a script. This method allows you to put command arguments directly in the exec parameter itself. However, while you may use symbolic variables in the exec, any symbolic variables contained within the script are not replaced during script execution.

    For security reasons, you cannot use this method with the SEND/RECV command's PREEXEC, EXEC or ACKEXEC parameters. Doing so generates an error: `CFTT97E cmd prefix not allowed in procedure execution for SEND and RECV commands`.

    To implement this method, preface the PREEXEC, EXEC or ACKEXEC value with "`cmd:`". For example:

    ```
    CFTSEND id=flow01, fname=myfile, exec="cmd:myscript.sh &PART &IDT &IDTU"
    ```

    To call a program, for example CFTUTIL, you can use a similar syntax as shown here:

    ```
    CFTSEND id=flow01, fname=myfile, exec="cmd:CFTUTIL
     end part=&PART, idt=&IDT, direct=SEND"
    ```

    ****Limitations Unix only****

    If a command is incorrect and cannot be executed, the transfer remains in the phasestep C. Possible reasons for this include:

    -   The command file name is too long
    -   The command file name does not point to a regular file
    -   The search permission is denied on a component of the command's path prefix
    -   The execute permission is denied
    -   The system does not support executing this type of file
    -   A loop exists in symbolic links encountered during resolution of the path or file argument

    > **Note**
    >
    > Tip  
    > Refer to man execve for an exhaustive list, since after a fork in the processes Transfer CFT does not retrieve the EXEC failure.

    Schedule processing
    -------------------

    You can use the Premindate/Premintime, Postmindate/Postmintime, and Ackmindate/Ackmintime parameters to schedule script processing activity. Additionally you can use prestate=Hold to wait for a START to start pre-processing.

    Throttle processing
    -------------------

    In some cases you may want to limit the number of scripts launched in parallel by {{< TransferCFT/suitevariablesTransferCFTName  >}} to reduce processing bottlenecks. To do so, set the UCONF `cft.server.max_processing_scripts` parameter to a positive integer to enable and control the number of executed processes.

    > **Note**
    >
    > Caution  
    > When using this parameter, every end-of-transfer procedure must notify Transfer CFT once the processing is complete. This can be done either via an END or KEEP command (in the case of an error). Failure to signal that processing is complete means that new procedures cannot start once the cft.server.max_processing_scripts value is reached.

    ```
    uconfset id= cft.server.max_processing_scripts
    , value=64
    ```

    > **Note**
    >
    > Note: This parameter does not apply to the execution of transfer error scripts.

    Commands in scripts
    -------------------

    ### End command

    The end command monitors the script completion. Depending on the parameter used (appstate, istate, and diagc) you can, for example, check the progression of the script. The end of processing is marked by a CFTUTIL END with `istate=no` (default).

    #### Define istate and appstate

    ****Example****

    ```
    CFTUTIL end part=&PART,idtu=&IDTU,istate=no,appstate="completed"
    ```

    The command CFTUTIL END can be use to set checkpoints in the script execution using the istate=yes (istate is an intermediate state) and APPSTATE value. Doing so allows you to see the step running the script in {{< TransferCFT/axwayvariablesComponentShortName  >}}.

    ****Example****

    ```
    CFTUTIL end part=&PART,idtu=&IDTU,istate=yes,appstate="step_1"CFTUTIL end part=&PART,idtu=&IDTU,istate=yes,appstate="step_2"
    ```

    #### Define DIAGC

    To create a more specific comment you can modify the DIAGC. The DIAGC used in preprocessing phase is reset when a transfer begins.

    ```
    CFTUTIL END part=&PART,idtu=&IDTU,DIAGC="intermediate checkpoint number 1 “
    ```

    #### Replace variable

    In your script you can also update values, for example the FNAME. However, the initial FNAME is lost in the catalog and replaced by the new one, and is only available in the log file as shown below.

    ****Example****

    ```
    CFTUTIL end part=&PART,idtu=&IDTU,FNAME=NEW_FNAME
    ...
    .....
    CFTR12I END Treated for USER MY_CFT : FNAME value was "pub/FTEST" and is now "NEW_FNAME"
    ```

    ### Stop

    When you execute a CFTUTIL HALT or CFTUTIL KEEP, you can set the DIAGP and DIAGC so that when you restart the script it executes specific actions depending on the DIAGP and DIAGC that you defined.

    ****Example****

    ```
    CFTUTIL HALT part=&PART,idtu=&IDTU,DIAGP=”Error 1”,DIAGC=”Connection lost”
    CFTUTIL KEEP part=&PART,idtu=&IDTU,DIAGP=”Error 404”,DIAGC=”File not found”
    ```

    ### Restart

    In your script, you can handle restart from intermediate steps checking the &APPSTATE value. So if the script fails for any reason, you can run a CFTUTIL HALT or CFTUTIL keep then using a CFTUTIL SUBMIT you can restart your script, which runs from the checkpoint that you set.

    Exa****m****ple  

    ```
    Go to &APPSTATE
    Step 1:
    if OK END part=&part, idtu=&idtu, appstate="step 1", istate=yes
    if not OK KEEP part=&part, idtu=&idtu, appstate="step 1", istate=yes &go to error
    Step 2:
    if OK END part=&part, idtu=&idtu, appstate="step 2", istate=yes
    if not OK KEEP part=&part, idtu=&idtu, appstate="step 2", istate=yes &go to error
    END:
    END part=&part, idtu=&idtu, istate=no & go to eof
    Error:
    WLOG MSG="script error at step &appstate"
    ```

    ### Define wait time for a restart

    You can change the maxduration for a transfer restart using the maxduration parameter in the START command.

    Command parameters
    ------------------

    ### SEND, CFTSEND

    Command
