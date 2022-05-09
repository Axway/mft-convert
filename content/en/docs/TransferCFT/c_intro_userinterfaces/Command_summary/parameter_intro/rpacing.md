{
    "title": "rpacing",
    "linkTitle": "rpacing",
    "weight": "2980"
}<span id="rpacing"></span>

### rpacing

#### CFTPROT

****[RPACING = { <span class="underline">32767</span> &#124; n } ]  {0...32767}****

Internal size, in Kbytes, between the two synchronization points for
the receiver (default = 32767). This value is negotiated with the spacing of the sender.
The value 0 indicates that there is no synchronization point.

If you are using Amazon S3 with Transfer CFT, please see [Amazon S3/Ceph cloud storage](../../../../app_integration_intro/amazon_s3)for RPACING recommendations and limitations.

[Return to Command index](../../)
