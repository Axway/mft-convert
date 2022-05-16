---

    title: Using  the LISTPKI command
    linkTitle: Using LISTPKI
    weight: 250

---
You can use the LISTPKI
command to display the local certificate database according to criteria
that you set in the command parameters. Additionally, you can use the default model or create a new model to customize the elements displayed in the output. By default, the LISTPKI command uses the`  dspcnf.xml` model file as a basis for the dispaly, which is located in `<installdir>/runtime/conf`.

## Display the local certificate database


| Parameter  | Description  |
| --- | --- |
| [CONTENT = BRIEF | FULL | DEBUG ] | Result display mode.<br/> • FULL: All contents display<br/> • DEBUG: All contents and additional general information display<br/> • BRIEF: a 79-character entry is displayed for each certificate. Additionally, when using the BRIEF value to display content:<br/> • Items display showing a clear parent/child relationship<br/> • If a certificate is marked as **Expired** (!), all of its children are labeled **Parent expired** (?)<br/> • If a certificate's parent is missing, the line displays either a question mark (**?**) if the issuer is filtered, or an exclamation mark (**!**) if the issuer is missing<br/> The <a href="#FMODEL%C2%A0d">FMODEL display example</a> features these elements.<br/> |
| [ID = {*, string1..8}] | Unique local identifier of the certificate(s) to be displayed.<br/> The * and ? wildcard characters are accepted for the ID parameter value. |
| [ INUM = {number0...99} ]  | Internal number for the intermediate certificates in an imported chain of certificates (in the PKI database).<br/> You can use this option to select a specific intermediate certificate. |
| [TYPE = ALL | USER | ROOT | INTER | KEY | CERT | | ENTITY ] | Type of certificate to display:<br/> • ALL: all certificates<br/> • USER: user certificates<br/> • ROOT: root authority certificates<br/> • INTER: intermediate authority certificates<br/> • KEY: list PKIKEY items<br/> • CERT: list ROOT, INTER, and USER certificates for PKICER only<br/> • ENTITY: any entity identifier created using PKIENTITY command |
| [STATE = ALL | ACT | INACT | EXPIRED] | Status of the certificates to display:<br/> • ALL: all statuses<br/> • ACT: all activated certificates<br/> • INACT: all deactivated certificates<br/> • EXPIRED: all expired certificates |
| FMODEL | The path to the file containing the models. If no model is found, the default format, which is the same as for the DISPLAY command, is used.<br/> <blockquote> **Note**<br/> Transfer CFT 3.7 and higher uses the dspcnf.xml model fileby default. To have the display format from a previous version, use FMODEL=NONE.<br/> </blockquote>  |
| ROOTCID  | The certificate authority ID. See an example in <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/rootcid">ROOTCID</a>.  |
| HELP  | Provides helps for the available fields for the different elements, or lists the available MODELS for the given file. Values include:<br/> • NONE (default)<br/> • FIELDS<br/> • MODELS |


<span id="CONTENT_BRIEF_Display"></span>

### CONTENT=BRIEF display (default format)

The following information is displayed for the CONTENT parameter BRIEF
value:

```
<span style="font-family: &quot;Courier New&quot;;"> 
Id.        Root    Type State
Exp.Date     Delivered to    Delivered by</span>
<span style="font-family: &quot;Courier New&quot;;"> 
4KL1CA     4KL1CA  R    INACT
28/07/2039   4k_l1_ca       
4k_root</span>
<span style="font-family: &quot;Courier New&quot;;"> 
4KL1CA1    4KL1CA1 R    ACT  
28/07/2039   4k_l1_ca       
4k_root</span>
<span style="font-family: &quot;Courier New&quot;;"> 
4KROOT     4KROOT  R    ACT  
28/07/2039   4k_root        
4k_root</span>
<span style="font-family: &quot;Courier New&quot;;">   
4K1L1EXP 4KROOT  U    ACT   14/10/2009 !
4k_l1_user2_exp 4k_root</span>
<span style="font-family: &quot;Courier New&quot;;">   
4KL1US1  4KROOT  U    ACT  
28/07/2039   4k_l1_user1     4k_root</span>
<span style="font-family: &quot;Courier New&quot;;">>      
5 PKICER selected</span>
 
<span style="font-family: &quot;Courier New&quot;;">Id.    
Certificates list</span>
<span style="font-family: &quot;Courier New&quot;;">ENTITY1 4KL1US1,
4K1L1EXP</span>
<span style="font-family: &quot;Courier New&quot;;">>      
1 PKIENTITY selected</span>
 
<span style="font-family: &quot;Courier New&quot;;">Id.             
S K Bits</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1USER1       
A x 4096</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1USER1KEYPRIV I x
4096</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1USER1KEYPUB 
A   4096</span>
<span style="font-family: &quot;Courier New&quot;;">>      
3 PKIKEY selected</span>
```
<span id="CONTENT_BRIEF_Display"></span>

### CONTENT=BRIEF display (former format, or no model)

```
<span style="font-family: &quot;Courier New&quot;;">Certificates:</span>
 
<span style="font-family: &quot;Courier New&quot;;">Id.         
Root         iNum T S C K E
Exp.Date   Delivered to  Delivered by</span>
<span style="font-family: &quot;Courier New&quot;;">------------
------------ ---- - - - - - ---------- ------------- -------------</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1CA      
4KL1CA            R I
x     28/07/2039 4k_l1_ca     
4k_root</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1CA1     
4KL1CA1           R A
x     28/07/2039 4k_l1_ca     
4k_root</span>
<span style="font-family: &quot;Courier New&quot;;">4KROOT      
4KROOT            R A
x     28/07/2039
4k_root       4k_root</span>
<span style="font-family: &quot;Courier New&quot;;">4K1L1EXP    
4KROOT            U A x
x ! 14/10/2009 4k_l1_user2_e 4k_root</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1US1     
4KROOT            U A x
x   28/07/2039 4k_l1_user1   4k_root</span>
 
<span style="font-family: &quot;Courier New&quot;;">Entities:</span>
 
<span style="font-family: &quot;Courier New&quot;;">Id.                             
Certificate list</span>
<span style="font-family: &quot;Courier New&quot;;">--------------------------------
---------------------------------</span>
<span style="font-family: &quot;Courier New&quot;;">ENTITY1                         
4KL1US1</span>
<span style="font-family: &quot;Courier New&quot;;">                                
4K1L1EXP</span>
<span style="font-family: &quot;Courier New&quot;;">Keys:</span>
 
<span style="font-family: &quot;Courier New&quot;;">Id.                             
S K Bits</span>
<span style="font-family: &quot;Courier New&quot;;">--------------------------------
- - ----</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1USER1                       
A x 4096</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1USER1KEYPRIV                
I x 4096</span>
<span style="font-family: &quot;Courier New&quot;;">4KL1USER1KEYPUB                 
A   4096</span>
```

****Id****

Identifier assigned to the certificate when it was imported into the
database.

****Root****

Identifier of the root certificate authority.

****Miscellaneous****

Miscellaneous certificate information (the letter only displays in when using the old format or no model):

- T: Type of Certificate: R for
    Root (Root Authority), I for Intermediate (Intermediate Authority),
    U for User
- S: Certificate state: A for
    active or I for inactive
- C: x denotes if the certificate
    is in the database
- K: x denotes if the private
    key associated with the certificate exists
- E: Certificate expired (!) or otherwise

****Exp. Date****

Expiry date of the certificate.

****Delivered to****

CN (Common name) attribute of the certificate user DN field.

****Delivered by****

CN (Common name) attribute of the certificate signer DN field.

<span id="CONTENT_FULL_Display"></span>

### CONTENT=FULL display (default display)

```
<span style="font-family: &quot;Courier New&quot;;"><FIELD  Cert
Id.             =
'4KL1US1'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Cert Type            =
'U'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Cert Root            =
'4KROOT'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Internal num         = ''</span>
<span style="font-family: &quot;Courier New&quot;;">       
Signer Id.           =
'4KROOT'</span>
<span style="font-family: &quot;Courier New&quot;;">       
State               
= 'ACT'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Serial number        = '45'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Delivered to         = '4k_l1_user1'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Delivered by         = '4k_root'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Issuer status        = 'missing'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Expired Before       = '29/07/2009'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Expired After        = '28/07/2039'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Comment             
= ''</span>
<span style="font-family: &quot;Courier New&quot;;">       
Owner's DN           =
'/C=FR/ST=HAUTS-DE-SEINE/L=PUTEAUX/O=CFT_SAMPLE/OU=CFT_L1_SAMPLE/CN=4k_l1_user1'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Signer's DN          =
'/C=FR/ST=HAUTS-DE-SEINE/L=PUTEAUX/O=CFT_SAMPLE/OU=CFT_SAMPLE/CN=4k_root'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Private RSA Key size = '4096'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Usage Key            =
'Digital Signature, Key Encipherment, Data Encipherment'</span>
<span style="font-family: &quot;Courier New&quot;;">       
Extended Usage Key   = 'TLS Web Server Authentication, TLS Web Client
Authentication'</span>
<span style="font-family: &quot;Courier New&quot;;">       
/></span>
<span style="font-family: &quot;Courier New&quot;;">>      
1 PKICER selected</span>
<span style="font-family: &quot;Courier New&quot;;">>      
1 PKICER without displayed issuer</span>
<span style="font-family: &quot;Courier New&quot;;">>      
0 PKIENTITY selected</span>
<span style="font-family: &quot;Courier New&quot;;">>      
0 PKIKEY selected</span>
```

### CONTENT=FULL display (former format, or no model)

```
<span style="font-family: &quot;Courier New&quot;;">         
Cert  id.     
ID         = 4KL1US1</span>
<span style="font-family: &quot;Courier New&quot;;">         
Cert  type    
TYPE       = USER</span>
<span style="font-family: &quot;Courier New&quot;;">         
Root  id.     
ROOT       = 4KROOT</span>
<span style="font-family: &quot;Courier New&quot;;">         
Internal num.  INUM       =</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Signer id.    
SID        = 4KROOT</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
State         
STATE      = ACT</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Serial number  SNUMB      = 45</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Delivered to   Us.CN      =</span>
<span style="font-family: &quot;Courier New&quot;;">         
4k_l1_user1</span>
<span style="font-family: &quot;Courier New&quot;;">         
</span>
<span style="font-family: &quot;Courier New&quot;;">         
Delivered by   Si.CN      =</span>
<span style="font-family: &quot;Courier New&quot;;">         
4k_root</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Certificat validity</span>
<span style="font-family: &quot;Courier New&quot;;">         
-------------------</span>
<span style="font-family: &quot;Courier New&quot;;">         
Expired Before :         29/07/2009
00:00:00</span>
<span style="font-family: &quot;Courier New&quot;;">         
Expired After  :         28/07/2039
23:59:59</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Comment       
COMMENT     =</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Owner's DN     OWNER'S DN  =</span>
<span style="font-family: &quot;Courier New&quot;;">         
/C=FR/ST=HAUTS-DE-SEINE/L=PUTEAUX/O=CFT_SAMPLE/OU=CFT_L1_SAMPLE/</span>
<span style="font-family: &quot;Courier New&quot;;">         
CN=4k_l1_user1</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">   
      Signer's DN    SIGNER'S DN =</span>
<span style="font-family: &quot;Courier New&quot;;">         
/C=FR/ST=HAUTS-DE-SEINE/L=PUTEAUX/O=CFT_SAMPLE/OU=CFT_SAMPLE/CN=</span>
<span style="font-family: &quot;Courier New&quot;;">         
4k_root</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Private Key type  RSA      =  4096 bits</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Usage
Key                 
=</span>
<span style="font-family: &quot;Courier New&quot;;">         
Digital Signature, Key Encipherment, Data Encipherment</span>
<span style="font-family: &quot;Courier New&quot;;"> </span>
<span style="font-family: &quot;Courier New&quot;;">         
Extended Usage Key         =</span>
<span style="font-family: &quot;Courier New&quot;;">         
TLS Web Server Authentication, TLS Web Client Authentication</span>
```

****Certificate id****

Identifier assigned to the certificate when it was imported into the
database.

****Certificate Type****

Identifier of the root certificate authority.

****Root id****

Identifier of the root certificate authority.

****Signer id****

Identifier of the certificate signer (issuer).

****State****

State of the certificate in the database (active or inactive).

****Serial Number****

Serial number of the certificate.

****Delivered to****

CN (Common name) attribute of the certificate user DN field.

****Delivered by****

CN (Common name) attribute of the certificate signer DN field.

****Expired before and after****

Period of validity of the certificate (start and end date )

****Comment****

Value assigned to the COMMENT parameter when the certificate was imported
into the database.

****Owner DN****

Value of the certificate user DN field.

****Signer DN****

Value of the certificate signer DN field.

<span id="INUM"></span>

### 

<span id="FMODEL d"></span>

### Filter using FMODEL example

You can use the FMODEL parameter to display only certain types of certificates; for example below we want to display the certificates that have the ACT state:

```
PKIUTIL LISTPKI CONTENT=BRIEF, STATE=ACT
```

Resulting in:

```
Id.      Root      Type   State Exp.Date   K   Delivered to Delivered by
EXPIRED  EXPIRED  ROOT   ACT   01/09/2015 !    4k_ca 4k_ca
EXPIRED  EXPIRED   USER   ACT   01/09/2030 ? x 4k_user 4k_ca
ROOT     ROOT      ROOT   ACT   22/07/2029   2k_root 2k_root
INTER1   ROOT      INTER  ACT   22/07/2029   2k_l1_ca 2k_root
? USER2  ROOT      USER   ACT  22/07/2029 x  2k_l3_user1 2k_l2_ca
USER1    ROOT      USER   ACT  22/07/2029 x  2k_l2_user1 2k_l1_ca
USER0    ROOT      USER   ACT  22/07/2029 x  2k_l1_user1 2k_root
> 7 Certificates selected
```

Key to indicators and symbols:

- The "EXPIRED" ROOT certificate displays the "!" symbol in the "K" column.
- The "EXPIRED" USER certificate displays the "?" symbol in the "K" column, indicating one if his issuer is expired.
- The "?" before their ID indicates that their issuer is not shown, although available.
- If a certificate displays an explanation mark "!" before the ID, it indicates that their issuer is not available (error).
