{
    "title": "commut",
    "linkTitle": "commut",
    "weight": "460"
}<span id="commut"></span>

### commut

#### CFTPART

**[COMMUT = {<span class="underline">YES</span> &#124; NO &#124; SERVER
&#124; PART }] **

Only applicable for a store and
forward site, and indicates the type of switching supported for this partner.

The following types are possible:

- YES: Enables the "store and forward" switching.
- SERVER: Indicates "VAN server switching".
- NO: Switching
    is refused for this partner.
- PART: The store
    and forward mode is forced in server mode if the recipient specified in
    the IPART parameter is not the final receiver.

See   [Store
and forward concepts.](../../../../concepts/transfer_command_overview/store_and_forward_mode_routing)

[Return to Command index](../../)
