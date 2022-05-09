{
    "title": "execsuba",
    "linkTitle": "execsuba",
    "weight": "960"
}### execsuba

CFTSEND, SEND

****[EXECSUBA = {LIST &#124; FILE &#124; <span class="underline">SUBF</span>}]****

Submit the acknowledgement
procedure when during a [send a group of files](../../../../concepts/send_command/send_group_of_files_cl) in both heterogeneous and homogeneous environments:

- LIST: trigger for acknowledgement
    procedure on the generic request, at the end of all transfers on the list
- FILE: trigger for the acknowledgement
    procedure for each request on the list and on the generic request
- SUBF: (default) trigger for acknowledgement procedure for each request on the list (except the generic request)

[Return to Command index](../../)
