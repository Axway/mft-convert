{
    "title": "spacing",
    "linkTitle": "spacing",
    "weight": "3260"
}<span id="spacing"></span>

### spacing

#### CFTPROT

**[SPACING     = { <span class="underline">32767</span>
&#124; n}]    ** {0..32767}

Interval in Kbytes between synchronization points being sent (default = 32767).

- This parameter is negotiated with the partner (RPACING parameter if
    {{< TransferCFT/axwayvariablesComponentShortName  >}}); the smallest value is selected as the interval
    between the synchronization points. The value 0 means that there are no
    synchronization points.
- During a transfer, the two PeSIT partners take a ‘synchronization point’
    whenever the sender sends a ‘SYN’ FPDU. A ‘SYN’ FPDU is sent every ‘n
    Kbytes’ of data sent (where ‘n’ is the smallest interval value negotiated
    in the connection phase - between SPACING and RPACING in the case of Transfer
    CFTs). Each synchronization point acknowledged may constitute a resumption
    point for a subsequent restart in the event a transfer is interrupted.

If you are using Amazon S3 with Transfer CFT, please see [Amazon S3/Ceph cloud storage](../../../../app_integration_intro/amazon_s3)for SPACING recommendations and limitations.

[Return to Command index](../../)
