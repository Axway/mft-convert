{
"title": "Product certificates",
"linkTitle": "Product certificates",
"weight": 80,
"date": "2019-11-25",
"description": "Details of signed certificates used by API Gateway, and sample certificates that are shipped with API Gateway and API Portal."
}

## API Gateway developer tools certificates

The Windows installer for the
developer tools has been signed using the private key associated with the following certificate:

| Owner    | Issuer           | Expiry           |
|----------|------------------|------------------|
| Owner: CN=Axway Inc, OU=Product Development, O=Axway Inc, L=Phoenix, ST=Arizona, C=US | Issuer: CN=Thawte Code Signing CA - G2, O="Thawte, Inc.", C=US | Valid from: Fri Dec 12 00:00:00 GMT 2014 until: Mon Dec 11 23:59:59 GMT 2017 |

## API Gateway JCE provider certificates

The product ships with its own JCE provider for interfacing to hardware security modules (HSMs) in a generic manner.  This provider is signed with the private key that corresponds to the public key in the following certificate:

| Owner       | Issuer                                  | Expiry         |
|-------------|-----------------------------------------|----------------|
| Owner: CN=Axway, OU=Java Software Code Signing, O=Sun Microsystems Inc | Issuer: CN=JCE Code Signing CA, OU=Java Software Code Signing, O=Sun Microsystems Inc, L=Palo Alto, ST=CA, C=US | Valid from: Tue Feb 12 00:53:53 GMT 2013 until: Fri Feb 16 00:53:53 GMT 2018 |

## API Gateway sample certificates

The following table lists the sample certificates with private keys that are shipped with the product.  These certificates are for test purposes only and must be replaced with production-ready certificates for a production environment.

| Sample certificate CN      | Issuing CA                 | Expires            |
|----------------------------|----------------------------|--------------------|
| Change this for production | Change this for production | 1st October, 2037  |
| Samples Test CA            | Samples Test CA            | 10th January, 2037 |
| Samples Test Certificate   | Samples Test CA            | 10th January       |

## API Gateway CA certificates

The following table lists the product-specific CA certificates that ship with the API Gateway's trusted certificate store.  The other default CA certificates in the certificate store are populated from the `cacerts` trust store in the JRE that is bundled with the product.

| CA common name | Issuing CA  | Expires           |
|----------------|-------------|-------------------|
| AxwayRootCA    | AxwayRootCA | 23rd August, 2042 |

To authenticate the product using SSL with other internal components, a domain certificate is generated using the `managedomain` utility.  This certificate is then used to sign the auto-generated per-instance certificates used in SSL communications between internal components, for example, an API Gateway instance and an Admin Node Manager.

While not necessary, you can change these certificates to more suitable enterprise-level CA-signed certificates, where available. [Configure Admin Node Manager high availability](/docs/apim_administration/apigtw_admin/admin_node_mngr/) describes how to use the `managedomain` utility to do this.

The following output shows the process whereby an initial domain-level key pair is generated for the Admin Node Manager:

```
Generating domain private key and certificate...
Generating private key...
Generating CSR for key...
Signing certificate for CSR...
Converting PEM files to P12...
Generating Node Manager private key and certificate...
Generating private key...
Generating CSR for key...
Signing certificate for CSR...
Converting PEM files to P12...
New Node Manager SSL certificate details:
   dname: CN=nodemanager-1,OU=group-1,DC=host-1
   expires: Sat Sep 22 12:13:04 BST 2114
   thumbprint: 7A:FE:7F:C0:D2:81:C6:39:F8:6C:FC:15:69:27:B9:E6:B1:ED:DC:09
   issuer dname: CN=Domain
   issuer thumbprint: FD:E9:7B:7C:DC:47:9E:A6:6A:93:C2:A2:52:CE:3A:46:3C:CE:0C:A8
Completed successfully.
You may now start the Node Manager on your newly registered host.
The system has placed your domain private key into directory '/nightly/15_10_14/apigateway/groups/certs/private'. Please backup and protect the contents of this directory.
```

Whenever a new API Gateway instance is created (using the `create_instance` command), the instance’s SSL certificate is signed by the domain level Admin Node Manager's SSL certificate that was generated in the sequence shown above.

```
> create_instance name Server1 group Group1
Requesting CSR from Admin Node Manager...
CSR received from Admin Node Manager.
Requesting signed certificate from Admin Node Manager...
Signed certificate received from Admin Node Manager.
Requesting Admin Node Manager to create new API Gateway...
New API Gateway SSL certificate details:
   dname: CN=instance-1,OU=group-2,DC=host-1
   expires: Sat Sep 22 12:20:13 BST 2114
   thumbprint: 67:76:D5:F7:A2:C5:64:10:40:75:85:4D:97:F5:CF:1A:8E:43:B0:CE
   issuer dname: CN=Domain
   issuer thumbprint: FD:E9:7B:7C:DC:47:9E:A6:6A:93:C2:A2:52:CE:3A:46:3C:CE:0C:A8
The new API Gateway 'Server1' in group 'Group1' has been successfully created and installed
```

## API Portal sample certificates

For API Portal, upon installation, a sample certificate is generated:

|Sample certificate CN|Issuing CA       |Validity|
|---------------------|-----------------|--------|
|Linux                |Certificate Shack|5 years |
