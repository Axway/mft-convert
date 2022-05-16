---

    title: execsuba
    linkTitle: execsuba
    weight: 970

---
### execsuba

CFTSEND, SEND

****\[EXECSUBA = {LIST | FILE | <u>SUBF</u>}\]****

Submit the acknowledgement
procedure when during a [send a group of files](../../../../concepts/using_the_send_command/send_group_of_files_cl) in both heterogeneous and homogeneous environments:

- LIST: trigger for acknowledgement
    procedure on the generic request, at the end of all transfers on the list
- FILE: trigger for the acknowledgement
    procedure for each request on the list and on the generic request
- SUBF: (default) trigger for acknowledgement procedure for each request on the list (except the generic request)

[Return to Command index](../../)
