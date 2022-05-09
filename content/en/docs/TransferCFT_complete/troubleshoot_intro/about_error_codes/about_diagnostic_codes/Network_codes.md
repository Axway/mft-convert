{
    "title": "Network  codes",
    "linkTitle": "Network codes",
    "weight": "360"
}NCR Common return code - Network interface
------------------------------------------

The NCR code corresponds to the "cr" code returned by the
network interface {{< TransferCFT/axwayvariablesComponentShortName  >}} functions, using
the formula: ****ncr = -(cr+20)****

Supply this value to Product Support for troubleshooting operations.

<span id="NCS"></span>

NCS System return code - Network interface
------------------------------------------

The value of this code depends on the type of network
and operating system. It corresponds to the "cs" code returned
by the network interface functions.

<span id="NCS___TCP_IP_System_Return_Codes"></span>

### NCS - TCP/IP System Return Codes

If the value of the code is less than 500 (decimal), that is 1F4 (hexadecimal),
it concerns the "errno" variable provided by the TCP/IP resource.
In this case, refer to the manufacturer's documentation for the meaning
of this code.


| Hexadecimal Code  | Decimal Code  | Meaning  |
| --- | --- | --- |
| 23a | 570 | Reception of a Define Resource request concerning an already registered resource |
| 23b | 571 | Reception of an Undefined Resource request for a resource with registered users |
| 23c | 572 | Reception of an Undefined Resource request for a resource with active connections |
| 23d | 573 | Maximum number of connections to a resource reached during a Connect Request |
| 244 | 580 | Cannot find an available port for the CFTTPRO polling socket |
| 245 | 581 | Reply to a synchronous request of incorrect length |
| 246 | 582 | CFTTPRO polling socket closed remotely during the TCP/IP server activation confirmation phase |
| 247 | 583 | Time-out while waiting for TCP/IP server activation confirmation message |
| 248 | 584 | Time-out while waiting for a synchronous request reply |
| 249 | 585 | CFTTPRO polling socket closed by remote end during the synchronous request reply wait phase |
| 26d | 621 | Invalid parameter received in response to a synchronous request (socket-based communication between CFTTCP and CFTTPRO) |
| 26e | 622 | Invalid parameter received in response to a synchronous request (queue-based communication between CFTTCP and CFTTPRO) |
| 26f | 623 | Flow control is active when it should not be |
| 270 | 624 | Host identifier invalid in a Define Resource request |
| 271 | 625 | Host identifier invalid in an Undefine Resource request |
| 272 | 626 | Invalid parameter to be sent in response to a synchronous request (Define Resource, Undefine Resource, Register Request or Deregister Request) |
| 273 | 627 | Invalid parameter to be sent in response to a synchronous request (Define Resource, Undefine Resource, Register Request or Deregister Request) |
| 274 | 628 | Host identifier invalid in a Register Request |
| 275 | 629 | Unknown request type provided by CFTTPRO |
| 276 | 630 | Define Resource request denied because maximum number of resources reached |
| 280 | 640 | Time-out when establishing the outgoing connection |
| 2b2 | 690 | Remote polling closed during datagram transmission |
| 2ef | 751 | Link socket between CFTTPRO and CFTTCPS closed (in the case of a socket connection) |
| 2f0 | 752 | Socket closed by remote end (L1 byte write phase) |
| 2f1 | 753 | Socket closed by remote end (L2 byte write phase) |
| 2f2 | 754 | Socket closed by remote end (data write phase) |
| 2f3 | 755 | Socket closed by remote end (L1 byte read phase) |
| 2f4 | 756 | Socket closed by remote end (L2 byte read phase) |
| 2f5 | 757 | Socket closed by remote end (data read phase) |
| 2f6 | 758 | Socket closed unexpectedly by remote end (L1 byte write phase) |
| 2f7 | 759 | Socket closed unexpectedly by remote end (L2 byte write phase) |
| 2f8 | 760 | Socket closed unexpectedly by remote end (data write phase) |
| 2f9 | 761 | Invalid status for read automaton (rdstate) |
| 2fa | 762 | Read error (L1 read phase) |
| 2fb | 763 | Read error (L2 read phase) |
| 2fc | 764 | Link socket closed by CFTTPRO |
| 3c0 | 960 | Register Request request for an unknown resource |
| 3c1 | 961 | Symbolic name unknown for a polling port |
| 3c2 | 962 | Use of a symbolic name for a polling port but this feature is not supported by the system |
| 3c3 | 963 | Host Internet address unknown |
| 3c4 | 964 | Host symbolic name unknown |
| 3c5 | 965 | Error in gethostbyname(): local host specification |
| 3c6 | 966 | Error in gethostbyname(): remote host specification |
| 3d6 | 982 | Undefine Resource request for an unknown resource |
| 3de | 990 | Register Request request for an unknown polling port name |
| 44e | 00001102 | Deregister Request request for an unknown reference: dynamic table allocation |
| 44f | 00001103 | De-register Request request for an unknown reference: dynamic allocation of the data area |
| 450 | 00001104 | Deregister Request request for an unknown reference: attribute string incorrect |
| 451 | 00001105 | Deregister Request request for an unknown reference: call parameter |
| 452 | 00001106 | Deregister Request request for an unknown reference: manager not running |
| 453 | 00001107 | Deregister Request request for an unknown reference: contexts still active |
| 454 | 00001108 | Deregister Request request for an unknown reference: table access denied |
| 455 | 00001109 | Deregister Request request for an unknown reference: no more available managers |
| 456 | 00001110 | Deregister Request request for an unknown reference: context does not exist |
| 457 | 00001111 | Deregister Request request for an unknown reference: end of table reached |
| 468  | 00001128  | ECONNREFUSED: The attempt to connect was rejected  |
| 4b2 | 00001202 | Context release problem during a Deregister Request: dynamic table allocation |
| 4b3 | 00001203 | Context release problem during a Deregister Request: dynamic allocation of the data area |
| 4b4 | 00001204 | Context release problem during a Deregister Request: incorrect attribute string |
| 4b5 | 00001205 | Context release problem during a Deregister Request: call parameter |
| 4b6 | 00001206 | Context release problem during a Deregister Request: manager not running |
| 4b7 | 00001207 | Context release problem during a Deregister Request: contexts still active |
| 4b8 | 00001208 | Context release problem during a Deregister Request: table access denied |
| 4b9 | 00001209 | Context release problem during a Deregister Request: no more managers available |
| 4ba | 00001210 | Context release problem during a Deregister Request: context does not exist |
| 4bb | 00001211 | Context release problem during a Deregister Request: end of table reached |
| 516 | 00001302 | Register context table scan error: dynamic table allocation |
| 517 | 00001303 | Register context table scan error: dynamic allocation of the data area |
| 518 | 00001304 | Register context table scan error: incorrect attribute string |
| 519 | 00001305 | Register context table scan error: call parameter |
| 51a | 00001306 | Register context table scan error: manager not running |
| 51b | 00001307 | Register context table scan error: contexts still active |
| 51c | 00001308 | Register context table scan error: table access denied |
| 51d | 00001309 | Register context table scan error: no more managers available |
| 51e | 00001310 | Register context table scan error: context does not exist |
| 51f | 00001311 | Register context table scan error: end of table reached |
| 57a | 00001402 | Connection context table scan error: dynamic table allocation |
| 57b | 00001403 | Connection context table scan error: dynamic data area allocation |
| 57c | 00001404 | Connection context table scan error: incorrect attribute string |
| 57d | 00001405 | Connection context table scan error: call parameter |
| 57e | 00001406 | Connection context table scan error: manager not running |
| 57f | 00001407 | Connection context table scan error: contexts still active |
| 580 | 00001408 | Connection context table scan error: table access denied |
| 581 | 00001409 | Connection context table scan error: no more managers available |
| 582 | 00001410 | Connection context table scan error: context does not exist |
| 583 | 00001411 | Connection context table scan error: end of table reached |
| 5de | 00001502 | Error in the search for an entry in the Register context table: dynamic table allocation |
| 5df | 00001503 | Error in the search for an entry in the Register context table: dynamic allocation of the data area |
| 5e0 | 00001504 | Error in the search for an entry in the Register context table: incorrect attribute string |
| 5e1 | 00001505 | Error in the search for an entry in the Register context table: call parameter |
| 5e2 | 00001506 | Error in the search for an entry in the Register context table: manager not running |
| 5e3 | 00001507 | Error in the search for an entry in the Register context table: contexts still active |
| 5e4 | 00001508 | Error in the search for an entry in the Register context table: table access denied |
| 5e5 | 00001509 | Error in the search for an entry in the Register context table: no more managers available |
| 5e6 | 00001510 | Error in the search for an entry in the Register context table: context does not exist |
| 5e7 | 00001511 | Error in the search for an entry in the Register context table: end of table reached |
| 642 | 00001602 | Error in the search for an entry in the connection context table: dynamic table allocation |
| 643 | 00001603 | Error in the search for an entry in the connection context table: dynamic data area allocation |
| 644 | 00001604 | Error in the search for an entry in the connection context table: incorrect attribute string |
| 645 | 00001605 | Error in the search for an entry in the connection context table: call parameter |
| 646 | 00001606 | Error in the search for an entry in the connection context table: manager not running |
| 647 | 00001607 | Error in the search for an entry in the connection context table: contexts still active |
| 648 | 00001608 | Error in the search for an entry in the connection context table: table access denied |
| 649 | 00001609 | Error in the search for an entry in the connection context table: no more managers available |
| 64a | 00001610 | Error in the search for an entry in the connection context table: context does not exist |
| 64b | 00001611 | Error in the search for an entry in the connection context table: end of table reached |
| 6a6 | 00001702 | Provider context (reference) invalid: dynamic table allocation |
| 6a7 | 00001703 | Provider context (reference) invalid: dynamic allocation of the data area |
| 6a8 | 00001704 | Provider context (reference) invalid: incorrect attribute string |
| 6a9 | 00001705 | Provider context (reference) invalid: call parameter |
| 6aa | 00001706 | Provider context (reference) invalid: manager not running |
| 6ab | 00001707 | Provider context (reference) invalid: contexts still active |
| 6ac | 00001708 | Provider context (reference) invalid: table access denied |
| 6ad | 00001709 | Provider context (reference) invalid: no more managers available |
| 6ae | 00001710 | Provider context (reference) invalid: context does not exist |
| 6af | 00001711 | Provider context (reference) invalid: end of table reached |
| 70a | 00001802 | Socket reference invalid: dynamic table allocation |
| 70b | 00001803 | Socket reference invalid: dynamic allocation of the data area |
| 70c | 00001804 | Socket reference invalid: incorrect attribute string |
| 70d | 00001805 | Socket reference invalid: call parameter |
| 70e | 00001806 | Socket reference invalid: manager not running |
| 70f | 00001807 | Socket reference invalid: contexts still active |
| 710 | 00001808 | Socket reference invalid: table access denied |
| 711 | 00001809 | Socket reference invalid: no more managers available |
| 712 | 00001810 | Socket reference invalid: context does not exist |
| 713 | 00001811 | Socket reference invalid: end of table reached |
| 76e | 00001902 | Provider context (index) invalid: dynamic table allocation |
| 76f | 00001903 | Provider context (index) invalid: dynamic allocation of the data area |
| 770 | 00001904 | Provider context (index) invalid: incorrect attribute string |
| 771 | 00001905 | Provider context (index) invalid: call parameter |
| 772 | 00001906 | Provider context (index) invalid: manager not running |
| 773 | 00001907 | Provider context (index) invalid: contexts still active |
| 774 | 00001908 | Provider context (index) invalid: table access denied |
| 775 | 00001909 | Provider context (index) invalid: no more managers available |
| 776 | 00001910 | Provider context (index) invalid: context does not exist |
| 777 | 00001911 | Provider context (index) invalid: end of table reached |
| 7d2 | 00002002 | Provider context (update) invalid: dynamic table allocation |
| 7d3 | 00002003 | Provider context (update) invalid: dynamic allocation of the data area |
| 7d4 | 00002004 | Provider context (update) invalid: incorrect attribute string |
| 7d5 | 00002005 | Provider context (update) invalid: call parameter |
| 7d6 | 00002006 | Provider context (update) invalid: manager not running |
| 7d7 | 00002007 | Provider context (update) invalid: contexts still active |
| 7d8 | 00002008 | Provider context (update) invalid: table access denied |
| 7d9 | 00002009 | Provider context (update) invalid: no more managers available |
| 7da | 00002010 | Provider context (update) invalid: context does not exist |
| 7db | 00002011 | Provider context (update) invalid: end of table reached |
| 836 | 00002102 | Provider context (delete) invalid: dynamic table allocation |
| 837 | 00002103 | Provider context (delete) invalid: dynamic allocation of the data area |
| 838 | 00002104 | Provider context (delete) invalid: incorrect attribute string |
| 839 | 00002105 | Provider context (delete) invalid: call parameter |
| 83a | 00002106 | Provider context (delete) invalid: manager not running |
| 83b | 00002107 | Provider context (delete) invalid: contexts still active |
| 83c | 00002108 | Provider context (delete) invalid: table access denied |
| 83d | 00002109 | Provider context (delete) invalid: no more managers available |
| 83e | 00002110 | Provider context (delete) invalid: context does not exist |
| 83f | 00002111 | Provider context (delete) invalid: end of table reached |
| 89a | 00002202 | Context release problem: dynamic table allocation |
| 89b | 00002203 | Context release problem: dynamic allocation of the data area |
| 89c | 00002204 | Context release problem: incorrect attribute string |
| 89d | 00002205 | Context release problem: call parameter |
| 89e | 00002206 | Context release problem: manager not running |
| 89f | 00002207 | Context release problem: contexts still active |
| 8a0 | 00002208 | Context release problem: table access denied |
| 8a1 | 00002209 | Context release problem: no more managers available |
| 8a2 | 00002210 | Context release problem: context does not exist |
| 8a3 | 00002211 | Context release problem: end of table reached |
| 8fe | 00002302 | Provider context invalid for a Ready To Receive Request: dynamic table allocation |
| 8ff | 00002303 | Provider context invalid for a Ready To Receive Request: dynamic allocation of the data area |
| 900 | 00002304 | Provider context invalid for a Ready To Receive Request: incorrect attribute string |
| 901 | 00002305 | Provider context invalid for a Ready To Receive Request: call parameter |
| 902 | 00002306 | Provider context invalid for a Ready To Receive Request: manager not running |
| 903 | 00002307 | Provider context invalid for a Ready To Receive Request: contexts still active |
| 904 | 00002308 | Provider context invalid for a Ready To Receive Request: table access denied |
| 905 | 00002309 | Provider context invalid for a Ready To Receive Request: no more managers available |
| 906 | 00002310 | Provider context invalid for a Ready To Receive Request: context does not exist |
| 907 | 00002311 | Provider context invalid for a Ready To Receive Request: end of table reached |
| 962 | 00002402 | Socket reference invalid: dynamic table allocation |
| 963 | 00002403 | Socket reference invalid: dynamic allocation of the data area |
| 964 | 00002404 | Socket reference invalid: incorrect attribute string |
| 965 | 00002405 | Socket reference invalid: call parameter |
| 966 | 00002406 | Socket reference invalid: manager not running |
| 967 | 00002407 | Socket reference invalid: contexts still active |
| 968 | 00002408 | Socket reference invalid: table access denied |
| 969 | 00002409 | Socket reference invalid: no more managers available |
| 96a | 00002410 | Socket reference invalid: context does not exist |
| 96b | 00002411 | Socket reference invalid: end of table reached |
| 9c6 | 00002502 | Provider context (asynchronous return) invalid: dynamic table allocation |
| 9c7 | 00002503 | Provider context (asynchronous return) invalid: dynamic allocation of the data area |
| 9c8 | 00002504 | Provider context (asynchronous return) invalid: incorrect attribute string |
| 9c9 | 00002505 | Provider context (asynchronous return) invalid: call parameter |
| 9ca | 00002506 | Provider context (asynchronous return) invalid: manager not running |
| 9cb | 00002507 | Provider context (asynchronous return) invalid: contexts still active |
| 9cc | 00002508 | Provider context (asynchronous return) invalid: table access denied |
| 9cd | 00002509 | Provider context (asynchronous return) invalid: no more managers available |
| 9ce | 00002510 | Provider context (asynchronous return) invalid: context does not exist |
| 9cf | 00002511 | Provider context (asynchronous return) invalid: end of table reached |
| a2a | 00002602 | Socket reference invalid for an Indication Datagram: dynamic table allocation |
| a2b | 00002603 | Socket reference invalid for an Indication Datagram: dynamic allocation of the data area |
| a2c | 00002604 | Socket reference invalid for an Indication Datagram: incorrect attribute string |
| a2d | 00002605 | Socket reference invalid for an Indication Datagram: call parameter |
| a2e | 00002606 | Socket reference invalid for an Indication Datagram: manager not running |
| a2f | 00002607 | Socket reference invalid for an Indication Datagram: contexts still active |
| a30 | 00002608 | Socket reference invalid for an Indication Datagram: table access denied |
| a31 | 00002609 | Socket reference invalid for an Indication Datagram: no more managers available |
| a32 | 00002610 | Socket reference invalid for an Indication Datagram: context does not exist |
| a33 | 00002611 | Socket reference invalid for an Indication Datagram: end of table reached |
| a8e | 00002702 | Search for a free context in the connection table: dynamic table allocation |
| a8f | 00002703 | Search for a free context in the connection table: dynamic allocation of the data area |
| a90 | 00002704 | Search for a free context in the connection table: incorrect attribute string |
| a91 | 00002705 | Search for a free context in the connection table: call parameter |
| a92 | 00002706 | Search for a free context in the connection table: manager not running |
| a93 | 00002707 | Search for a free context in the connection table: contexts still active |
| a94 | 00002708 | Search for a free context in the connection table: table access denied |
| a95 | 00002709 | Search for a free context in the connection table: no more managers available |
| a96 | 00002710 | Search for a free context in the connection table: context does not exist |
| a97 | 00002711 | Search for a free context in the connection table: end of table reached |
| af2 | 00002802 | Search for a free context in the Register context table: dynamic table allocation |
| af3 | 00002803 | Search for a free context in the Register context table: dynamic allocation of the data area |
| af4 | 00002804 | Search for a free context in the Register context table: incorrect attribute string |
| af5 | 00002805 | Search for a free context in the Register context table: call parameter |
| af6 | 00002806 | Search for a free context in the Register context table: manager not running |
| af7 | 00002807 | Search for a free context in the Register context table: contexts still active |
| af8 | 00002808 | Search for a free context in the Register context table: table access denied |
| af9 | 00002809 | Search for a free context in the Register context table: no more managers available |
| afa | 00002810 | Search for a free context in the Register context table: context does not exist |
| afb | 00002811 | Search for a free context in the Register context table: end of table reached |
| bb9 | 00003001 | Error during TCP/IP process creation: incorrect task name |
| bba | 00003002 | Error during TCP/IP process creation: definition error |
| bbb | 00003003 | Error during TCP/IP process creation: insufficient memory |
| bc1 | 00003009 | Error during TCP/IP process creation: general problem |
| c1d | 00003101 | Queue acquisition error (queue-based communication between CFTTCP and CFTTPRO): invalid queue |
| c1e | 00003102 | Queue acquisition error (queue-based communication between CFTTCP and CFTTPRO): time-out |
| c1f | 00003103 | Queue acquisition error (queue-based communication between CFTTCP and CFTTPRO): queue deleted |
| c25 | 00003109 | Queue acquisition error (queue-based communication between CFTTCP and CFTTPRO): general problem |
| c80 | 00003200 | CFTTCP initialization error (tmbeg) |
| ce5 | 00003301 | CFTTCP initialization error (tmstat): incorrect task |
| ced | 00003309 | CFTTCP initialization error (tmstat): general problem |
| dad | 00003501 | Queue error in Data Indication: invalid queue reference |
| dae | 00003502 | Queue error in Data Indication: counter capacity exceeded |
| daf | 00003503 | Queue error in Data Indication: control message or data too large |
| db0 | 00003504 | Queue error in Data Indication: invalid mode |
| db5 | 00003509 | Queue error in Data Indication: general problem |
| db7 | 00003511 | Queue error in Connect Reject Indication: invalid queue reference |
| db8 | 00003512 | Queue error in Connect Reject Indication: counter capacity exceeded |
| db9 | 00003513 | Queue error in Connect Reject Indication: control message or data too large |
| dba | 00003514 | Queue error in Connect Reject Indication: invalid mode |
| dbf | 00003519 | Queue error in Connect Reject Indication: general problem |
| dc1 | 00003521 | Queue error in Release Indication: invalid queue reference |
| dc2 | 00003522 | Queue error in Release Indication: counter capacity exceeded |
| dc3 | 00003523 | Queue error in Release Indication: control message or data too large |
| dc4 | 00003524 | Queue error in Release Indication: invalid mode |
| dc9 | 00003529 | Queue error in Release Indication: general problem |
| dcb | 00003531 | Queue error in Connect Accept Indication: invalid queue reference |
| dcc | 00003532 | Queue error in Connect Accept Indication: counter capacity exceeded |
| dcd | 00003533 | Queue error in Connect Accept Indication: control message or data too large |
| dce | 00003534 | Queue error in Connect Accept Indication: invalid mode |
| dd3 | 00003539 | Queue error in Connect Accept Indication: general problem |
| dd5 | 00003541 | Queue error in Indication Datagram: invalid queue reference |
| dd6 | 00003542 | Queue error in Indication Datagram: counter capacity exceeded |
| dd7 | 00003543 | Queue error in Indication Datagram: control message or data too large |
| dd8 | 00003544 | Queue error in Indication Datagram: invalid mode |
| ddd | 00003549 | Queue error in Indication Datagram: general problem |
| ddf | 00003551 | Queue error in Connect Indication: invalid queue reference |
| de0 | 00003552 | Queue error in Connect Indication: counter capacity exceeded |
| de1 | 00003553 | Queue error in Connect Indication: control message or data too large |
| de2 | 00003554 | Queue error in Connect Indication: invalid mode |
| de7 | 00003559 | Queue error in Connect Indication: general problem |
| de9 | 00003561 | Queue error in Ready To Receive Indication: invalid queue reference |
| dea | 00003562 | Queue error in Ready To Receive Indication: counter capacity exceeded |
| deb | 00003563 | Queue error in Ready To Receive Indication: control message or data too large |
| dec | 00003564 | Queue error in Ready To Receive Indication: invalid mode |
| df1 | 00003569 | Queue error in Ready To Receive Indication: general problem |
| 0000eb68  | 00060264  | ECONNREFUSED: The attempt to connect was rejected  |

