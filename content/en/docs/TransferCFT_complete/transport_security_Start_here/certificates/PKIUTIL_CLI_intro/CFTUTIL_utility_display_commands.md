{
    "title": "CFTUTIL utility display commands",
    "linkTitle": "CFTUTIL display command",
    "weight": "210"
}This topic describes the Transfer CFT display command for Transfer CFT transport security.

<span id="Displaying_the_CFTSSL_commands"></span>

### Displaying the CFTSSL commands

The LISTPARM command accepts the SSL value for the TYPE parameter. This
option is used to display the contents of a CFTSSL command.

The CFTSSL commands are displayed for the TYPE parameter ALL value.

<span id="Displaying_the_catalog__Content_FULL_"></span>

### Displaying the catalog (Content=FULL)

The LISTCAT command, CONTENT=FULL option, displays transport security
information. This data is listed at the end of the display, after the
Receiver application (RAPPL) field.

     Receiver application RAPPL =  
1     SSL mode SSLMODE =  
2     SSL authority policy SSLAUTH =  
3     SSL cipher type SSLCIPH =  
4     SSL profile ID SSLPROF =  
5     SSL remote CA alias SSLRMCA =  
6     SSL remote CN SSLRMCN =  
7     SSL local user alias SSLUSER =  
8     SSL certificate filename SSLCFNAM =  
     \*  
9     SSL user parameter SSLPARM =  
     \*SSL FOR CLIENT

The sections are only specified if the transfer was performed in an
SSL session.

The following table contains comments on the new sections.

****Section definitions for LISTCAT CONTENT=FULL****


| Section  | Description  |
| --- | --- |
| 1  | SSL session direction used for the transfer.<br/> C signifies client and S signifies server.  |
| 2  | SSL session authentication mode used for the transfer.<br/> • S signifies that only the server was authenticated.<br/> • B signifies that the client and server were authenticated.<br/> • A signifies that the anonymous mode has been implemented.  |
| 3  | Suite negotiated for the SSL session.<br/> This suite is set to one of the values from the suites supported by Transfer CFT (1, 2, 4, 5, 9, 10 or 47).  |
| 4  | Identifier of the CFTSSL command used to negotiate the session parameters.  |
| 5  | Certificate identifier of the authority that signed the certificate presented by the remote partner.  |
| 6  | 32-character remote user certificate CN (Common Name) field.<br/> If the certificate CN length exceeds 32 characters, it is truncated.  |
| 7  | Identifier of the user certificate used locally for authentication by the remote partner.  |
| 8  | Physical name of the file in which the certificate presented by the remote partner was recorded.  |
| 9  | Value of the PARM parameter in the CFTSSL command used to negotiate the session parameters.  |

