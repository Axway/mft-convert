{
    "title": "Amazon S3/Ceph cloud storage ",
    "linkTitle": "Amazon S3/Ceph cloud storage",
    "weight": "170"
}You can use Transfer CFT with [Amazon S3](https://aws.amazon.com/s3/) cloud storage to store and retrieve large numbers of files to better manage enterprise big data.

{{< TransferCFT/suitevariablesTransferCFTName  >}} implements Amazon S3 services using the AWS SDK for C++ (Amazon Web Services Software Development Kit). The Transfer CFT file process uses the AWS SDK to directly deposit or access files (binary) from S3 storage, without saving data on a local hard drive.

Limitations
-----------

- Available on Linux x86-64 and Windows x86-64 exclusively.
- Does not support FACTION=RETRYRENAME.
- The following parameter settings are ignored: WFNAME, FACTION=ERASE or RENAME (S3 creates its own version of the file)
- You cannot access a file on S3 storage from the Transfer CFT Copilot UI.
- Folder monitoring is not supported with Amazon S3 Storage.
- Transfer CFT Windows presently only supports HTTP proxies (not HTTPS proxies).
- When writing a file to S3 storage with PACING, the partner-negotiated value of the RPACING and SPACING parameters, the following restrictions apply:
    -   The minimum negotiated value must be 5120 KiB (5 MiB).
    -   The maximum size of the transferred file is proportional to the negotiated value of PACING, which is approximately 335 GB (~ 320 GiB). This is calculated by PACING KiB upload x 10,000 parts = 32,767 KiB (where 32,767 \*1024 = 33,553,408 bytes).

Setup procedure
---------------

1. For SSL connections to S3 storage, libCURL requires a path to the CA certificates bundle to authenticate the peer. Set this path in the UCONF `ssl.certificates.ca_cert_bundle` parameter.
1. Access keys are used for the AWS credentials, so each user accessing S3 services must have an account allowing access to S3 services. Define the users and access key information in UCONF. For example:  
    ```
    CFTUTIL uconfset id=aws.credentials, value="
    account1 account2
    "
    CFTUTIL uconfset id=aws.credentials.account1.access_key_id, value="20_characters_string"
    CFTUTIL uconfset id=aws.credentials.account1.secret_access_key, value="40_characters_string"
    CFTUTIL uconfset id=aws.credentials.account2.access_key_id, value="20_characters_string"
    CFTUTIL uconfset id=aws.credentials.account2.secret_access_key, value="40_characters_string"
     
    To check, enter:
    CFTUTIL listuconf id=aws.credentials\*
    ```

Parameter descriptions
----------------------

The following tables describe Transfer CFT's Amazon S3-related parameters.


| Parameter  | Type  | Description  |
| --- | --- | --- |
| ssl.certificates.ca_cert_bundle  | string  | Path to the CA certificate bundle. This path can point to either a file containing the CA certificates (for example, <code>/etc/ssl/certs/ca-certificates.crt</code>) or to a directory containing the CA certificates (for example, <code>/etc/ssl/certs/</code>), which are stored individually with their filenames in a hash format.<br/> You can refer to the [cacert.pem](https://curl.haxx.se/docs/manpage.html#--cacert)). |


**Credentials**

Credential parameters identify who is making a request and whether access is allowed.


| Parameter  | Type  | Description  |
| --- | --- | --- |
| aws.credentials  | node  | List of [STORAGEACCOUNT](#storageaccount) values where each STORAGEACCOUNT consists of an access key pair.  |
| aws.credentials.&lt;storageaccount&gt;.access_key_id  | string  | Access key ID, a 20-character, alphanumeric sequence. |
| aws.credentials.&lt;storageaccount&gt;.secret_access_key  | passwd  | Secret access key, a 40-character sequence. |


**Region**


| Parameter  | Type  | Description  |
| --- | --- | --- |
| aws.credentials.&lt;storageaccount&gt;.region  | string  | Region to use; this value overrides the parsing of the WORKINGDIR and the config/env settings.<br/> When using the endpoint format for WORKINGDIR, you can use this parameter to set the AWS Signature Version 4 region to something other than the default (us-east-1). |


**Server-side encryption**

Server-side encryption type with Amazon S3.


| Parameter  | Type  | Description  |
| --- | --- | --- |
| aws.credentials.&lt;storageaccount&gt;.encryption_type  | string  | Type of server-side encryption to use:<br/> • None: No encryption on Amazon S3 objects<br/> • sse-s3: Server-side encryption with AES 256<br/> • sse-kms: Server-side encryption with Key Management Service<br/> See the [example](#globally_encrypt) for details on encrypting files. |
| aws.credentials.&lt;storageaccount&gt;.encryption_key_id  | string  | Key identifier for SSE-KMS encryption use; you must provide the full ID of the encryption key for the server. |


**Access Control List**

The ACL policy to apply to files when uploading a file to AWS. Please refer to the [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#canned-acl) for details.


| Parameter  | Type  | Description  |
| --- | --- | --- |
| aws.credentials.&lt;storageaccount&gt;.acl  | string  | Type of ACL policy to apply to files when uploading a file to AWS:<br/> • private<br/> • public-read<br/> • public-read-write<br/> • aws-exec-read<br/> • authenticated-read<br/> • bucket-owner-read<br/> • bucket-owner-full-control |


**Proxy parameters**

Connect to AWS S3 through an HTTP proxy for file uploads/downloads.


| Parameter  | Type  | Description  |
| --- | --- | --- |
| aws.credentials.&lt;storageaccount&gt;.proxy_scheme  | string  | Proxy scheme to use (HTTP or HTTPS).  |
| aws.credentials.&lt;storageaccount&gt;.proxy_host  | string  | Proxy IP address or FQDN.  |
| aws.credentials.&lt;storageaccount&gt;.proxy_port  | string  | Proxy port.  |
| aws.credentials.&lt;storageaccount&gt;.proxy_username  | string  | Proxy user name.  |
| aws.credentials.&lt;storageaccount&gt;.proxy_password  | password  | Proxy password.  |


****<span id="Default_key_access_pair"></span>Default access key pair behavior****

If you did not define a `storageaccount `value in the CFTSEND/CFTRECV objects, when you connect to AWS services the AWS SDK checks in the `$HOME/.aws/credentials `file for a profile and credentials.

You can enter your access key pair information in this file using the following format:

```
[default]
access_key_id = YOUR_KEY
secret_access_key = YOUR_SECRET
```

Creating send and receive definitions
-------------------------------------

You must include the following parameters in your [CFTSEND/CFTRECV](../../c_intro_userinterfaces/command_summary) definitions for S3 storage:


| Parameter<span id="storageaccount"></span>  | Type  | Description  |
| --- | --- | --- |
| fname  | string (key)  | The fname field corresponds to the S3 services key.  |
| workingdir  | string  | There are two supported formats. For either, the workingdir field must start with <code>s3://</code> and be followed by the designated items in the order listed:<br/> • <code>s3://bucket.region</code><br /> • the bucket name<br/> • a period (.)<br/> • the region<br/> <br/> • <code>s3://http[s]://endpoint[:port]/bucket</code><br /> • http:// or https:// for secure communication<br/> • the endpoint, which can be an IP address or the server's hostname<br/> • a colon (:) and port (if not using the default of 80 for HTTP, 443 for HTTPS)<br/> • a slash (/)<br/> • the bucket name<br/>  |
| storageaccount  | string  | Points to the access key identifier(s) and the access key secret(s) stored in UCONF. See also [storageaccount](../../c_intro_userinterfaces/command_summary/parameter_intro/storageaccount).  |


You can use the following example as a basis for configuring a CFTSEND and CFTRECV:

```
CFTSEND id = S3_READ,
fname = pub/FTEST,
workingdir = s3://my-cft-test.eu-west-1,
storageaccount=account1
 
CFTRECV id = S3_WRITE,
fname = pub/&IDF.&IDTU.RCV,
workingdir = s3://https://s3.eu-west-1.amazonaws.com/my-cft-test,
storageaccount=account2
```
<span id="Using"></span>

Using AWS EC2 instance profiles to access S3 storage
----------------------------------------------------

To enhance the use of {{< TransferCFT/axwayvariablesComponentLongName  >}} in large AWS deployments, you can use an IAM role to manage access for applications that run on an EC2 instance. To implement, you need an IAM role that allows access to S3 defined in an IAM instance profile, which is then associated with the EC2 instance. For example, Transfer CFT could be hosted on an EC2 instance that inherits its permissions from the associated instance profile to access the S3 bucket.

No configuration is required on {{< TransferCFT/axwayvariablesComponentLongName  >}}, you can simply point the CFTSEND or CFTRECV WORKINGDIR field to the desired S3 endpoint and leave the STORAGEACCOUNT definition empty.

****Example****

```
CFTRECV id = S3_WRITE_EC2,
fname = pub/&IDF.&IDTU.RCV,
workingdir = s3://my-cft-test.eu-west-1
```

Transfer CFT supports mixing an IAM role from an IAM instance profile with multiple IAM users who have their own access keys.

<span id="Use"></span>

Using Ceph storage with S3
--------------------------

Transfer CFT can write objects to the Ceph Storage Cluster using the [Ceph Object Gateway](https://docs.ceph.com/docs/master/radosgw) and the S3 compatible API. The Ceph platform stores data as objects in storage pools on a distributed storage cluster.

To use the Ceph Storage Cluster via its S3 API, follow the S3 storage instructions on this page. Additionally, when implementing:

- Use the `s3://http[s]://endpoint[:port]/bucket` format for the workingdir.

    Example: `s3://http://radosgw_address.net:7480/my_bucket`, where 7480 is the CivetWeb default port on which the Ceph Object Gateway is running.

- You can change the port and the SSL enabled option for the Ceph configuration.
- Create the credentials, and add the access key and secret access key to the UCONF `aws.credentials.<storage_account>` parameters.

More information:

- AWS4 signature, available as of Jewel v10, must be supported by Ceph. We used Luminous v12 for testing purposes. Please refer to [ACTIVE RELEASES](https://docs.ceph.com/docs/master/releases/#active-releases).

Use case examples
-----------------

In this section, *S3* may refer to either Amazon S3 storage or the Ceph Storage Cluster, which is S3-compatible.

### Transfer CFT receives a file and stores it on S3

In this use case, Transfer CFT  receives a file from a partner over the PeSIT or SFTP protocol, and stores it on either Amazon S3 or Ceph.

![](/Images/TransferCFT/S3_write_file.png)

In UCONF, set the S3 credentials for the receiving Transfer CFT:

```
uconfset id=aws.credentials, value='<ceph_user>'
uconfset id=aws.credentials.ceph_user.access_key_id, value=<key_id>
uconfset id=aws.credentials.ceph_user.secret_access_key, value=<access_key>
```

Configure the CFTRECV object to write to the S3 storage:

```
CFTRECV id=CEPH_WRITE, fname=pub/&IDF.&IDTU.RCV, workingdir=s3://http://radosgw_address.net:7480/my_bucket, storageaccount=<ceph_user>
```

After the partner sends a file, you can check the log for transfer details.

### Transfer CFT reads a file from S3 and sends to a partner

In this use case, Transfer CFT reads a file from S3 and sends it over PeSIT or SFTP to a partner.

![](/Images/TransferCFT/S3_read_file.png)

Configure the UCONF credentials for the Ceph Object Storage if you have not already done so:

```
uconfset id=aws.credentials, value='<ceph_user>'
uconfset id=aws.credentials.ceph_user.access_key_id, value=<key_id>
uconfset id=aws.credentials.ceph_user.secret_access_key, value=<access_key>
```

Create the CFTSEND template, and send a file that is stored on S3 to a Transfer CFT partner.

```
CFTSEND id=ceph_send, workingdir=s3://http://radosgw_address.net:7480/<my_bucket>, storageaccount=<ceph_user>send part=paris,idf=ceph_send,fname=pub/FTEST
```

After sending a file to the partner, you can check the log for transfer details.

### Enable server side encryption

<span id="globally_encrypt"></span>You can activate file encryption when uploading files using either the sse-s3 or sse-kms encryption type. Use CFTUTIL and the uconfset command as follows:

```
uconfset id=aws.credentials.<storageaccount>.encryption_type, value='sse-s3'
```

When using sse-kms, you must additionally enter your key identifier:

```
uconfset id=aws.credentials.<storageaccount>.encryption_type, value='sse-kms'
uconfset id=aws.credentials.h<storageaccount>.encryption_key_id, value=<key_id>
```

### Set the ACL policy

You can set the Access Control List (ACL) policy for files that you upload to AWS S3 as follows:

```
uconfset id=aws.credentials.<storageaccount>.acl, value='<ACL policy>'
```
<span id="How"></span>

### Troubleshooting

This section provides information on how to troubleshoot errors that you may encounter when implementing S3 with Transfer CFT. Note that you can troubleshoot not only in Transfer CFT, but also using the AWS command line tool, as outlined below.

### Transfer CFT checks

****CFTF30W AWS S3 error (-1): Unable to connect to endpoint****

This error may occur for one of the following reasons:

- The connection to the Amazon server is firewalled, or the DNS is misconfigured.

<!-- -->

- Ensure that the server can access the Amazon server and can resolve the address. For instance, ping the server to verify:

<!-- -->

- On Linux, the SSL certificates auto-detection failed. Use the UCONF `ssl.certificates.ca_cert_bundle` parameter to point to current certificates.
- The region is invalid for the bucket. Ensure that the `workingdir `parameter of the send/recv command is valid.

****CFTF30W AWS S3 error (13/HTTP 403): Permission denied - No response body****

Access to the file was denied for the given storageaccount.

Ensure that the credentials defined in the UCONF `aws.credentials.<storageaccount>.access_key_id `and `secret_access_key` parameters are valid, and that the user is authorized to access the file on the S3 server.

<span id="AWS"></span>

### AWS CLI

To help resolve errors, you can use the AWS CLI tool to verify that the system can connect to the S3 storage, and to check that the user has permission to read the keys in the bucket. Please refer to the instructions provided in the AWS documentation: <https://docs.aws.amazon.com/cli/>

**Examples**

To list objects in a bucket on Amazon S3:

```
aws --region eu-west-1 s3api list-objects --bucket my-cft-test
```

To list objects in a Ceph storage:

```
aws --endpoint-url http://radosgw_address.net:7480 s3api list-objects --bucket my_bucket
```

To create a bucket:

```
aws --endpoint-url http://radosgw_address.net:7480 s3api create-bucket --bucket my_bucket
```
