{
    "title": "Configure optional  features",
    "linkTitle": "Configure optional features",
    "weight": "250"
}<span id="Create a Transfer CFT PKI file D43PKI"></span><span id="kanchor37"></span>

Create a Transfer CFT PKI file D43PKI
-------------------------------------

The <span id="kanchor38"></span>PKI file contains all information necessary to use the SSL component:

- Certification authorities
- Certificates
- Private keys

The examples were created using OPENSSL software, from the Institut Fédéral Suisse de Technologie (Swiss Federal Institute of Technology).

The D43PKI step creates a VSAM KSDS file and initializes it with the supplied examples. To update the PKIFILE file, you can use the CFTPKI utility model.

It is important to follow these guidelines:

- The supplied examples are designed expressly to check that the SSL component is operating correctly. Do NOT use these examples for any other purpose.
- Take all appropriate security measures to guarantee the confidentiality of the security data.
- The usage of data ciphering is subject to legal restrictions that vary from country to country.

<span id="_Toc236186612"></span>

Use a SAF based PKI 
--------------------

System Authorization Facility (SAF) based PKI offers a more secured SSL. The optional SAF method uses RACF, or the equivalent, and an optional cryptographic coprocessor to increase data security. To enable the SAF mode of operation you must:

- Enable the IBM Crypto Express 2 coprocessor, if available
- Configure the IBM ICSF program to use a secured PKDS database

<!-- -->

- Install and configure the option for Transfer CFT

<!-- -->

- Define RACF

<!-- -->

- Define the CFTSSL PARM fields or ROOTCID/USERCID fields

#### Enabling the Transfer CFT PKI system D47SYST

To enable the Transfer CFT PKI system, access the CONFIG file. The job responsible for enabling the PKI system is D47SYST.

#### Managing the PKI database

A sample named RACDCERT is delivered in the cftv2.INSTALL library. This RACDCERT show how to create a RING named XFBCFT, and to add test certificates: AXWMFTCA and AXWMFTUS. Additionally, the sample adds RACF authorizations for the Transfer CFT userid.

> **Note**
>
> Note: Transfer CFT no longer delivers sample certificates.

For information on SAF compatible security products, such as ACF/2 or TOP-SECRET, refer to the product-supplied documentation.

For information on how to manage certificates using RACF, refer to the IBM documentation SA22-7687, Security Server RACF Command Language Reference. For example, if you are not using an IBM Crypto Express coprocessor, you must remove the PCICC(\*) parameter of the RACDCER ADD command.

#### CFTSSL parameter changes for a SAF based PKI

SAF certificate management is based on:

- A RING used to store one or more certificates (RACDECRT RING)

<!-- -->

- The USER that owns the certificates (RACDCERT ID)

Both the RING and OWNER are passed to the system from the PARM field or ROOTCID/USERCID fields of the CFTSSL definitions. Code CFTSSL using one of the two following methods:

```
CFTSSL         ID= …,
ROOTCID = 'certificate authority_1’,
USERCID = 'Local user certificate’,
PARM=’RING=ring_name,OWNER=userid’
```

or

```
CFTSSL ID= ...,
ROOTCID = '(certificate authority_1@userid_1@ring_name_1 ,
certificate authority_2@userid_2@ring_name_2)',
USERCID = 'Local user certificate@userid@ring_name',
```

By default the delimiter between the id, the owner, and the ring name is '@' (x’7C’), but you can define a different delimiter using the PARM field of CFTSSL card.

**Example**

```
PARM='DLM=$'
```

**Variables description**

- ring_name: A string value matching the RACDCERT RING parameter
- userid: A 1 to 7 character string value that matches the RACDCERT ID parameter

The PARM field length is limited to 64 bytes. You must select a RING NAME that conforms to the 64 bytes restriction.

#### Security considerations for a SAF based PKI

Required tasks for the correct exit functioning include:

- The PKI exit sets or switches the security environment for the user defined as OWNER=userid. To set the security environment, Transfer CFT must be started and APF authorized

<!-- -->

- You must add SAF security definitions for OWNER=userid, in the SAF CLASS named FACILITY. Setting the definitions involves a number of IRR.DIGCERT.\*.

The SAF definitions are described in the IBM documentation SA22-7691 Security Server RACF Callable Services, chapter IRRSDL00.

Transfer CFT will perform the DATAGETFIRST/DATAGETNEXT and the CHECKSTATUS operations.

<span id="Communication server C32XMEM"></span><span id="kanchor39"></span>

Communication server C32XMEM
----------------------------

The communication between the APPLICATION and Transfer CFT is done through a media file, CFTCOM, or a TCPIP interface. On a mainframe an additional media is available, as described in Using a communication server.

****Related topics****

- [Migrating Transfer CFT z/OS]()
- [Installing a patch](../../../upgrade_prereqs_zos/c_update_zos/t_install_patch_zos)
