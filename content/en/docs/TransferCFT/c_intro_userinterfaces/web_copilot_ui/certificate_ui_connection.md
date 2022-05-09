{
    "title": "How to generate a certificate chain",
    "linkTitle": "Generate a certificate for the UI",
    "weight": "190"
}You can use the following steps to create a full certificate chain to secure the user interface's web browser connection. You can, for example, use a current version of OpenSSL to generate the certificates.

Remember to replace paths, certificate names, and passwords with your own values. Additionally, The Common Name (CN) must be the same as the web address you use to connect to the user interface. For example, if `CN=myca` then access the user interface with `https://myca:<apirest_port>/cft/ui`.

1. Generate a Certificate Authority (CA).

      
    ```
    openssl req -new -x509 -subj "/C=FR/O=mycompany/OU=myunit/CN=myca" -days 3650 -sha256 -addext "keyUsage = Certificate Sign, CRL Sign" -newkey rsa:2048 -keyout myca-key.pem -out myca.pem -passin pass:myca -nodes
    ```

1. Generate the end-entity certificate sign request.

      
    ```
    openssl req -new -subj "/C=FR/O=mycompany/OU=myunit/CN=myserver.mycompany.com" -newkey rsa:2048 -keyout mycert-key.pem -out mycert-req.pem -passin pass:mycert -nodes
    ```

1. Create extension file for the end-entity certificate sign request. In place of the DNS:&lt;hostname&gt;, use the system or computer hostname.

      
    ```
    echo basicConstraints = CA:FALSE >mycert-req.ext
    echo keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment >>mycert-req.ext
    echo subjectAltName = DNS:<hostname> >>mycert-req.ext
    ```

1. Generate and sign the end-entity certificate.

      
    ```
    openssl pkcs12 -export -chain -CAfile myca.pem -inkey mycert-key.pem -in mycert.pem -passin pass:mycert -passout pass:mycertp12 -out mycert.p12
    ```

1. Import the `myca.pem` certificate in your web browser to add the CA certificate as trusted authority. The steps will depend on the type of web browser you use.
