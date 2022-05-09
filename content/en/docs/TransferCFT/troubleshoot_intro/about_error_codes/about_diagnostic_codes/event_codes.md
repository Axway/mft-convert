{
    "title": "DIAGP: Event  codes",
    "linkTitle": "Event codes",
    "weight": "340"
}This code is common to all protocols. When a value is specific to a
protocol, the indication appears in brackets.

For the PeSIT protocol, this code forms part of the "eNNsNN"-type
PeSIT DIAGP.

Event Codes for all protocols


| Code | Meaning  |
| --- | --- |
| 00 | VFABORTD - Transfer abort request by {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| 01 | VFCAND - Transfer interrupt request by {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| 02 | VFCANR - Response to a transfer interrupt indication |
| 03 | VFCHKD - Request to set a synchronization point |
| 04 | VFCHKR - Response to a synchronization point indication |
| 05 | VFCLOSD - Request to close file |
| 06 | VFCLOSRN - Negative response to a file close indication |
| 07 | VFCLOSRP - Positive response to a file close indication |
| 08 | VFCONRN - Negative response to a connect indication |
| 09 | VFCREAD - Request to create a file |
| 10 | VFCREARN - Negative response to a file create indication |
| 11 | VFCREARP - Positive response to a file create indication |
| 12 | VFDATAD - Request to send data |
| 13 | VFDSELD- Request to select a file |
| 14 | VFDSELRN - Negative response to a select indication |
| 15 | VFDSELRP - Positive response to a select indication |
| 16 | VFDTNDD - End of data request |
| 17 | VFECOND - Request to connect in WRITE mode |
| 18 | VFECONRP - Positive response to a connect indication |
| 19 | VFLCOND - Request to connect in READ mode |
| 20 | VFLCONRP - Positive response to a connect indication |
| 21 | Not used |
| 22 | VFOMSGD - Request to send a message |
| 23 | VFOMSGRN - Negative response to a message indication |
| 24 | VFOMSGRP - Positive response to a message indication |
| 25 | VFOPEND - Request to open a file |
| 26 | VFOPENRN - Negative response to a file open indication |
| 27 | VFOPENRP - Positive response to a file open indication |
| 28 | VFRDY - Internal induction for the automaton table |
| 29 | VFRDYD - Internal induction for the automaton table |
| 30 | VFREADD - Request to read a file |
| 31 | VFREADRN - Negative response to a read request |
| 32 | VFREADRP - Positive response to a read request |
| 33 | VFRELD - Network close request |
| 34 | VFRELR - Response to network close request |
| 35 | Not used |
| 36 | VFRSTAR - Response to a re-synchronization request |
| 37 | VFSELD - Request to select a file |
| 38 | VFSELRN - Negative response to a select request |
| 39 | VFSELRP - Positive response to a select request |
| 40 | VFTRD - Request to start up a new transfer |
| 41 | VFTRNDD - Request to end a transfer |
| 42 | VFTRNDR - Response to an end of transfer request |
| 43 | VFTRNDRN - Negative response to an end of transfer |
| 44 | VFTRNDRP - Positive response to an end of transfer |
| 45 | VFWRITD - Request to write a file |
| 46 | VFWRITRN - Negative response to a write request |
| 47 | VFWRITRP - Positive response to a write request |
| 48 | VLOGD - Request to send a pre-logon message |
| 49 | VLOGRN - Negative response to a pre-logon message |
| 50 | VLOGRP - Positive response to a pre-logon message |
| 51 | Not used |
| 52 | VNCONRP (NETWORK) - Positive response to an incoming call |
| 53 | Not used |
| 54 | VNRELC (NETWORK) - Confirmation of network outage |
| 55 | VNRELI (NETWORK) - Network outage indication |
| 56 | VRABORT (PESIT) - Reception of an ABORT FPDU |
| 57 | VRACK (PESIT) - Reception of a pre-logon acknowledgment |
| 58 | VRACON (PESIT) - Reception of an AckCONNECT FPDU |
| 59 | VRACREAN (PESIT) Reception of a negative AckCREATE FPDU |
| 60 | VRACREAP (PESIT) Reception of a positive AckCREATE FPDU |
| 61 | VRACRFN (PESIT) - Reception of a negative AckCRF FPDU |
| 62 | VRACRFP (PESIT) - Reception of a positive AckCRF FPDU |
| 63  | VRADSELN (PESIT) - Reception of a negative AckDESELECT FPDU  |
| 64 | VRADSELP (PESIT) - Reception of a positive AckDESELECT FPDU |
| 65 | VRAIDT (PESIT) - Reception of an AckCANCEL FPDU |
| 66 | VRAOMSGN (PESIT) - Reception of a negative AckMESSAGE FPDU |
| 67 | VRAOMSGP (PESIT) Reception of a positive AckMESSAGE FPDU |
| 68 | VRAORFN (PESIT) - Reception of a negative AckORF FPDU |
| 69 | VRAORFP (PESIT) - Reception of a positive AckORF FPDU |
| 70 | VRAREADN (PESIT) - Reception of a negative AckREAD FPDU |
| 71 | VRAREADP (PESIT) - Reception of a positive AckREAD FPDU |
| 72 | VRARESY (PESIT) - Reception of an AckRESYN FPDU |
| 73 | VRASELN (PESIT) - Reception of a negative AckSELECT FPDU |
| 74 | VRASELP (PESIT) - Reception of a positive AckSELECT FPDU |
| 75 | VRASY (PESIT) - Reception of a synchronization acknowledgment |
| 76 | VRATRNDN (PESIT) - Reception of a negative AckTRANSFER.END FPDU |
| 77 | VRATRNDP (PESIT) - Reception of a positive AckTRANSFER.END FPDU |
| 78 | VRAWRITN (PESIT) - Reception of a negative AckWRITE FPDU |
| 79 | VRAWRITP (PESIT) - Reception of a positive AckWRITE FPDU |
| 80 | VRCON (PESIT) - Reception of a CONNECT FPDU |
| 81 | VRCREA (PESIT) - Reception of a CREATE FPDU |
| 82 | VRCRF (PESIT) - Reception of a CRF (Close Remote File) FPDU |
| 83 | VRDSEL (PESIT) - Reception of a DESLECT FPDU |
| 84 | VRDTF (PESIT) - Reception of a DATA FPDU |
| 85 | VRDTFDA (PESIT) - Reception of a DATA (Start) FPDU |
| 86 | VRDTFFA (PESIT) - Reception of a DATA (End) FPDU |
| 87 | VRDTFMA (PESIT) - Reception of an end of DATA (Middle) FPDU |
| 88 | VRDTND (PESIT) - Reception of an end of DATA FPDU |
| 89 | VRIDT (PESIT) - Reception of a CANCEL FPDU |
| 90 | VRLOG (PESIT) - Reception of a pre-logon message |
| 91 | VRNACCN (NETWORK) - Outgoing call refused |
| 92 | VRNACCP (NETWORK) - Outgoing call accepted |
| 93 | VRNCON (NETWORK) - Incoming call indication |
| 94 | VROMSG (PESIT) - Reception of a MESSAGE FPDU |
| 95 | VRORF (PESIT) - Received an ORF (Open Remote File) FPDU |
| 96 | VRRCON (PESIT) - Received a Release CONNECT FPDU |
| 97 | VRRDY (NETWORK) - Network ready-to-send indication |
| 98 | Not used |
| 99 | VRREAD (PESIT) - Reception of a READ FPDU |
| 100 | VRREL (PESIT) - Reception of a RELEASE FPDU |
| 101 | VRRELCF (PESIT) - Reception of a RELEASE Confirm FPDU |
| 102 | VRRESY (PESIT) - Reception of a RE-SYNCHRONIZATION FPDU |
| 103 | VRSEL (PESIT) - Reception of a SELECT FPDU |
| 104 | VRSY (PESIT) - Reception of a SYNCHRONIZATION FPDU |
| 105 | VRTRND (PESIT) - Reception of a TRANSFER.END FPDU |
| 106 | VRWRIT (PESIT) - Reception of a WRITE FPDU |
| 107 | VVTIMO - Time-out |
| 108 | VFDATA1 - Internal induction for the automaton table |
| 109 | VRDTF1 - Internal induction for the automaton table |
| 110 | VVERCRC - Detection of a CRC error |
| 111 | VVERR - Detection of an inconsistent FPDU |
| 112 | VIABORTS - Internal induction for the automaton table |
| 113 | VIABORTC - Internal induction for the automaton table |
| 114 | VIRSTR - Internal induction for the automaton table |
| 115 | VINACCN - Internal induction for the automaton table |
| 116 | Not used |
| 117 | Not used |
| 118 | Not used |
| 119 | Not used |
| 120 | Not used |
| 121 | Not used |
| 122 | VRODMSG (PESIT) - Received a MESSAGE (Start) FPDU |
| 123 | VROMMSG (PESIT) - Received a MESSAGE (Middle) FPDU |
| 124 | VROFMSG (PESIT) - Received a MESSAGE (End) FPDU |
| 125 | Not used |
| 126 | VFCD (ODETTE) - Change direction request |
| 127 | VRASSID (ODETTE) Received an SSID (acknowledgment) |
| 128 | VRCD (ODETTE) - Reception of a CD (Change Direction) |
| 129 | VRCDT (ODETTE) - Reception of a CDT (Set Credit) |
| 130 | VREERP (ODETTE) - Reception of an EERP (End To End Response) |
| 131 | VREFNA (ODETTE) - Reception of an EFNA (End File Negative Answer) |
| 132 | VREFPA (ODETTE) - Reception of an EFPA (End File Positive Answer) |
| 133 | VREFID (ODETTE) - Reception of an EFID (End File Identification) |
| 134 | VRESID (ODETTE) - Reception of an ESID (End Session Identification) |
| 135 | Not used |
| 136 | Not used |
| 137 | VRRSSID (ODETTE) - Received an SSID (Start Session Identification) FPDU |
| 138 | VRRTR (ODETTE) - Received an RTR (Ready To Receive) |
| 139 | VRSFNA (ODETTE) - Reception of an SFNA (Start File Negative Answer) |
| 140 | VRSFPA (ODETTE) - Reception of an SFPA (Start File Positive Answer) |
| 141 | VRSFID (ODETTE) - Reception of an SFID (Start File Identification) |
| 142 | VRSSID (ODETTE) - Reception of an SFID (Start File Identification) |
| 143 | VRSSRM (ODETTE) - Reception of an SSRM (Start Session Ready Message) |
| 144 | VIESID (ODETTE) - Internal induction for the automaton table |
| 145 | VISSID (ODETTE) - Internal induction for the automaton table |
| 146 | VIRREAD (ODETTE) - Internal induction for the automaton table |
| 147 | VIDSEL (ODETTE) - Internal induction for the automaton table |
| 148 | VIABORTCD (ODETTE) - Internal induction for the automaton table |

