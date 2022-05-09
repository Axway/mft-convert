{
    "title": "execsub",
    "linkTitle": "execsub",
    "weight": "950"
}<span id="execsub"></span>

### execsub

#### CFTSEND, SEND

****[EXECSUB = { LIST &#124; FILE &#124; SUBF }]****

Submits an [end-of-transfer](../../../../concepts/about_transfer_processing/procedure_examples)
procedure during a [send a group of files](../../../../concepts/send_command/send_group_of_files_cl) in both heterogeneous and homogeneous environments:

- LIST: Triggers the end-of-transfer
    procedure for the generic (parent) request
- FILE: Triggers the end-of-transfer
    procedure for each request on the list and for the generic request (parent and child transfers)
- SUBF: Triggers the end-of-transfer procedure for child transfers only

[Return to Command index](../../)
