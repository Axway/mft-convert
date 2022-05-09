{
    "title": "PKIUTIL  error codes",
    "linkTitle": "PKIUTIL error codes",
    "weight": "290"
}The following table lists the codes for the PKIU26E PKICER_Error ( msg
{150nn/0}) type error message.

PKIUTIL error codes


| Code  | Contains  | Meaning  |
| --- | --- | --- |
| 15001  | Command not authorized  | The user is not authorized to use this command  |
| 15003  | PKI file opening error  | Error opening the file  |
| 15004  | PKI invalid file opening mode  | Error opening the file with the request mode  |
| 15005  | PKI internal error  | Type of record unknown  |
| 15006  | PKI record already exist  | The record already exists  |
| 15007  | PKI invalid parameter  | Incorrect value for one of the fields in the command  |
| 15008  | PKI Record writing error  | Error writing to the file  |
| 15009  | Not enough memory to proceed this command  | Memory allocation error  |
| 15010  | Command OK  | Record complete  |
| 15011  | No record found  | Record cannot be found  |
| 15012  | PKI record reading error  | Error reading the file  |
| 15013  | PKI file end  | End of file reached  |
| 15014  | PKI Record suppression error  | Error deleting a record  |
| 15015  | Certificate or Key reading error  | Error opening a file to be imported (key or certificate)  |
| 15016  | Authorization failed  | Authorization problem  |
| 15017  | PKI record not found  | Internal datafile empty  |
| 15018  | Certificate Length too long  | Certificate size too large  |
| 15019  | Error creating extract file fout  | Creation of the extract file fout failed  |
| 15020  | Error extracting a PKI certificate  | Creation of the file containing the certificate failed  |
| 15021  | PKI internal error  | Internal error  |
| 15022  | PKI file integrity error  | Certificate internal datafile integrity error  |
| 15024  | Habilitation opening error  | Error initializing the security system  |
| 15025  | Parsing error on Certificate or Key  | Certificate or key interpretation error  |
| 15026  | PKI decipher error  | Error decrypting the certificate or key  |
| 15027  | PKI cipher error  | Encryption error  |
| 15028  | PKI decipher error  | Decryption error  |
| 15029  | PKI sealing (MD5) error  | MD5 sealing error  |
| 15030  | PKI key file reading error  | Error reading the password file  |
| 15031  | IKNAME or INAME parameter mandatory  | iname or ikname parameter mandatory  |
| 15032  | PKI record type error  | Certificate type unknown  |
| 15034  | Private and Public key incompatible  | Public and private keys incompatible  |
| 15035  | CA Certificate not found  | Root authority certificate cannot be found  |
| 15036  | Signature check failed  | Signature invalid  |
| 15038  | PKI record conflict: existing entity  | Cannot insert/modify the certificate (User, Root, Inter, or Other type), because there is already an entity using the same ID in the PKI database  |
| 15039  | PKI record conflict: existing certificate  | Cannot insert/modify the entity, because there is already a certificate using the same ID in the PKI database  |
| 15099  | Format not yet supported  | Certificate type not supported  |

