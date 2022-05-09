{
    "title": "Time-saving shortcuts",
    "linkTitle": "How to use autocomplete",
    "weight": "160"
}{{< TransferCFT/suitevariablesTransferCFTName  >}} offers several commands and options that enable you to work more quickly and efficiently. The following sections describe:

- [Using autocompletion](#Using)
- [Using the `check `command](#Using2)
- [Using the previous/next shortcut](#Previous)

<span id="Using"></span>

Using bash autocompletion
-------------------------

**UNIX only**

To simplify the use of Transfer CFT commands, you can use the bash autocompletion feature when working in interactive mode. Bash autocompletion is valid for `CFTUTIL`, `PKIUTIL`, and the `cft` commands. This feature intuitively provides commands and available parameters along with a brief description. Additionally, for certain parameters Bash autocompletion proposes a list of possible parameter values, either static or dynamic, depending on the parameter.

- [Keyboard shortcuts](#Special)
    -   [Use the Bash autocompletion keys](#Auto-com)
    -   [UCONF parameter specifics](#UCONF%C2%A0pa)

> **Note**
>
> Note: Bash autocompletion does not display if you enter a command directly after the CFTUTIL keyword. For example, no commands are suggested if you type: CFTUTIL SEND

> **Note**
>
> Note: You require bash version 2.0.5 or higher.  Additionally, Bash must be loaded prior to loading the Transfer CFT profile.

<span id="Special"></span>

### Keyboard shortcuts

The CFTUTIL utility uses the following keys as shortcuts when entering a command.


| Key | Action |
| --- | --- |
| Left Arrow | Move the cursor one character to the left. |
| Right Arrow | Move the cursor one character to the right. |
| Ctrl + Left Arrow | Move the cursor one word to the left. |
| Ctrl + Right Arrow | Move the cursor one word to the right. |
| Home | Move the cursor to the first character of the command. |
| End | Move the cursor after the last character of the command. |
| Del | Remove the character at the cursor position. |
| Backspace | Remove the character before the cursor. |
| Insert | Switch between insertion and replace mode. |
| Escape | Erase the command (Windows only). |
| Enter | Submit the command. |
| Ctrl + C | CFTUTIL hard exit (not recommended). |


<span id="Auto-com"></span>

### Use the Bash autocompletion keys

You can use the **Tab** and **Shift + Tab** keys to display and scroll through available commands, parameters, and values. If you are not familiar with available commands, begin by pressing **Tab** at the `CFTUTIL `prompt; the first command, SEND, displays along with a brief description.

![](/Images/TransferCFT/Auto_completion_in_CFTUTIL.png)

- Press **Tab** again to display the next available command, RECV. The command list displays commands in order of importance and typical usage. Continue to press **Tab** until the command that you want displays. To reverse the order, use **Shift** + **Tab**.

![](/Images/TransferCFT/Auto_completion_in_CFTUTIL_1.png)

- Once you have selected a command press **Space** to validate the selection.
- Next, press **Tab** to display the first parameter for that command. Use the same method as for the command - **Tab**, **Shift** + **Tab** , or the initial characters.
- If the parameter description is prefaced by an asterisk (\*), the parameter is mandatory. After selecting the parameter, type the equal sign (=) to continue.

![](/Images/TransferCFT/Auto_completion_in_CFTUTIL_2.png)

- Once you have selected a parameter value type a comma (,) to valid your choice and go to the next parameter. If there are no additional parameters, press Enter to execute the command.

![](/Images/TransferCFT/Auto_completion_in_CFTUTIL_3.png)

<span id="UCONF pa"></span>

### UCONF parameter specifics

To display UCONF parameters, from `ID=` the autocomplete works by completing categories until the period (.) separator is reached.

- For example, cft.multi_node.cftcom.dispatcher_policy is comprised of 4 categories (cft, multi_node, cftcom, and dispatcher_policy).
- Once the ID is complete, a space is appended to the parameter name to indicate the end of the parameter.

![](/Images/TransferCFT/Auto_completion_in_CFTUTIL_4.png)

<span id="Using2"></span>

Using the check command
-----------------------

The CFTUTIL `CHECK `command validates the coherence of parameters, partners, and the Transfer CFT PKI database.

The syntax is:

```
CHECK CONTENT=<span class="underline">BRIEF</span>&#124;FULL, FOUT=FileName
```

The `CHECK CONTENT=BRIEF` (default) command verifies that:

- All the referenced objects exist
- Each CFTPART has an associated CFTTCP
- There is at least one CFTPARM
- The used certificates have not expired

Any encountered errors are displayed in the console, and we highly recommend that you fix them before starting Transfer CFT.

The `CHECK CONTENT=FULL, FOUT=FileName` command also checks that:

- All objects are used
- No CRONTAB is empty
- Each CFTPROT is used by at least one CFTPART
- There is just one CFTPARM

Where:

- The FOUT option sends the `check `results to a file instead of displaying in the console.

You can use the `check`command with `cftinit `and `cftupdate `using the following syntax:

- `cftinit -check file`
- `cftupdate -check file`

Here, the `-check` option is equivalent to running `CFTUTIL CHECK `at the end of a successful cftinit or cftupdate .

<span id="Previous"></span>

Using the previous/next shortcut
--------------------------------

You can use the **Up/Down Arrow** keys as a shortcut to recall the previous or next command.

<span id="Partial"></span>

Partial word recognition
------------------------

To retrieve a command you know, begin by typing the first characters of the command. CFTUTIL returns only the commands starting with those characters. For example type LI and press **Tab**, the commands LISTCAT, LISTLOG, LISTCOM display. You can then use the **Up/Down Arrow** keys to scroll the list.

<span id="Command-"></span>

### Command-history settings

Use the following uconf parameters to manage the command-history settings.


| Parameter | Default value | Description |
| --- | --- | --- |
| cft.readline.history_size | 500 | Maximum number of commands that you can store. |
| cft.readline.enable | Yes | Save and retrieve the commands from disk. |
| cft.readline.history_fname |  • Win: %APPDATA%\cft\CftutilHistory.txt<br/> • Unix: $(HOME)/.cft_history | Name of the file containing the command history. |


<span id="Modify"></span>

### Modify the command list

The file containing the list of commands is a text file that you can edit or remove, and its location is defined in the `cft.readline.history_fname` parameter.
