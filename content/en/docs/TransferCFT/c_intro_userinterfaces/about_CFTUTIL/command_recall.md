{
    "title": "Use the previous/next command ",
    "linkTitle": "Use previous/next command ",
    "weight": "180"
}You can use the **Up/Down Arrow** keys as a shortcut to recall the previous or next command.

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
