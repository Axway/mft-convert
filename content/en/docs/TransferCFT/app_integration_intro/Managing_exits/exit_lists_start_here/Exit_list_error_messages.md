{
    "title": "Exit  list error messages",
    "linkTitle": "Exit list error messages",
    "weight": "330"
}The error messages stored in the {{< TransferCFT/axwayvariablesComponentShortName  >}} log
have the following syntax:

CFTT60I PART= &part IDF=
&idf IDT= &idt, &messageLog


| DiagP<br /> Catalog  | &amp;messageLog  | Comment  |
| --- | --- | --- |
| - - -  | Exit List V2.4.0  | Indicates the start of execution of the exit-list program  |
| EXL RESV  | Err_parameter RESERV  | Error in RESERV parameter of CFTEXIT command<br/> RESERV = 16384 is correct  |
| EXL FILE  | Err_open criteria file  | Selection criteria file opening error  |
| EXL CATA  | Err_open catalog file  | Catalog opening error  |
| EXL LECT  | Err_read catalog file  | Catalog read error  |
| EXL SYST  | Err_system  | Local system error, cause unknown, generally a problem of memory allocation  |


****Related topics****

- [Messages
    and error codes](../../../../troubleshoot_intro/messages_and_error_codes_start_here)
