---

    title: Start and stop the Copilot server
    linkTitle: Start and stop the Transfer CFT UI (Copilot) server
    weight: 190

---
When <span class="code">`am.type=passport`</span> or <span class="code">`am.type=cg`</span>, the user that connects to the Transfer CFT Copilot server must be defined in the system as well as in PassPort/{{< TransferCFT/PrimaryCGorUM  >}}.

## Start the Copilot server

This section describes how to start the Transfer CFT Copilot server via either a menu or command. For more information on calling menus, see <a href="../#Transfer" class="MCXref xref">Transfer CFT menu usage</a>.

****Menu****

1. Access the *Transfer CFT* <span class="italic_in_para" style="font-style: italic;">**Main Menu**</span>.  
    In the Main Menu enter the command <span class="code">`cft`</span> and press <span class="bold_in_para">****Enter****</span> to open the Transfer CFT menu.
1. Enter **1** to access **Common CFT commands**.
1. Select option <span class="span_5" style="font-weight: bold;">****1 Start Copilot****</span>. The *Copilot server* menu is displayed.  

****Command****

Execute: <span class="code">`COPSTART `</span>

## Stop the Copilot server

This section describes how to stop the Transfer CFT Copilot server via either a menu or command.

****Menu****

1. Access the *Transfer CFT* <span class="italic_in_para" style="font-style: italic;">**Main Menu**</span>.  
    In the Main Menu enter the command <span class="code">`cft`</span> and press <span class="bold_in_para">****Enter****</span> to open the Transfer CFT menu.
1. Enter **1** to access **Common CFT commands**.
1. Select option <span class="span_5" style="font-weight: bold;">****2**** </span><span class="span_5" style="font-weight: bold;">****Stop Copilot****</span>.  
    Only the server waiting for a connection is stopped. Other servers that users have logged onto are shut down when the user logs off, or after a network timeout.

****Command****

Execute: <span class="code">`COPSTOP `</span>

## Configure the Copilot server

Use the UCONFSET commands to modify the configuration if you need to modify the Transfer CFT Copilot server.
