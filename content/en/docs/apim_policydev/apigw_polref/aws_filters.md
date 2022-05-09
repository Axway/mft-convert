{
"title": "AWS filters",
  "linkTitle": "AWS filters",
  "weight": 80,
  "date": "2019-10-17",
  "description": "Upload data to Amazon S3 or send messages to Amazon SQS."
}

For more information on Amazon Simple Storage Service, see [Amazon S3](http://aws.amazon.com/s3/).

For detailed information on Amazon Web Services integration, see the *AWS Integration Guide* available from Axway Support.

For more information on configuring AWS client settings in Policy Studio, see
[Configure AWS client settings](/docs/apim_policydev/apigw_gw_instances/general_aws_poller/#configure-aws-client-settings).

## Upload to Amazon S3 filter

Amazon Simple Storage Service (S3) is an online storage web service that you can use to store and retrieve any amount of data. API Gateway acts as a client to S3 and can upload data to S3. You can use the **Upload to Amazon S3**
filter to upload data to Amazon S3.

Configure the following settings on the **Upload to Amazon S3**
window:

**Name**:
Enter a suitable name for the filter to display in a policy.

### S3 AWS settings

**AWS Credential**:
Click the browse button to select your AWS security credentials (API key and secret) for Amazon S3.

**Region**:
Select the region in which to store your data. You can choose from the following options:

* US East (Northern Virginia)
* US West (Oregon)
* US West (Northern California)
* EU (Ireland)
* Asia Pacific (Singapore)
* Asia Pacific (Tokyo)
* Asia Pacific (Sydney)
* South America (Sao Paulo)
* US GovCloud

**Client settings**:
Click the browse button to select the AWS client configuration to be used by API Gateway when connecting to Amazon S3.

### S3 settings

**Bucket name**:
Enter the name of the bucket in which to store the data. To create a new bucket with the specified name, click the **Create**
option.

**Object key**:
Enter the object key for the object to be stored. Alternatively, you can enter a selector that is expanded at runtime.

**Encryption key**:
Click the browse button to select an encryption key for the object.

**How to store**:
Select how to store the object. You can choose from the following options:

* Standard – This is the standard S3 storage option.
* Reduced Redundancy – This is a storage option within Amazon S3 for storing non-critical, reproducible data at lower levels of redundancy than standard storage.
* Glacier – This is a low-cost storage option for data archival.

## Send to Amazon SQS filter

Amazon Simple Queue Service (SQS) is a hosted message queuing service for distributing messages amongst machines. API Gateway acts as a client to SQS and can send messages to SQS. You can use the **Send to Amazon SQS**
filter to send messages to an SQS queue.

Configure the following settings on the **Send to Amazon SQS**
window:

**Name**:
Enter a suitable name for the filter to display in a policy.

### SQS AWS settings

**AWS Credential**:
Click the browse button to select your AWS security credentials (API key and secret) for Amazon SQS.

**Region**:
Select the region in which to access the SQS service. You can choose from the following options:

* US East (Northern Virginia)
* US West (Oregon)
* US West (Northern California)
* EU (Ireland)
* Asia Pacific (Singapore)
* Asia Pacific (Tokyo)
* Asia Pacific (Sydney)
* South America (Sao Paulo)
* US GovCloud

**Client settings**:
Click the browse button to select the AWS client configuration to be used by API Gateway when connecting to Amazon SQS.

### SQS Send message settings

Configure the following settings on the **Send Message**
tab:

**Queue name**:
Enter the name of the queue to send the message to. The default name is `publishQueue`. To create a new queue with the specified name, click the **Create**
option.

**Send the message payload**:
Select this option to send the message payload to the queue.

**Or send the value of the attribute below to SQS**:
Select this option to send the value of an attribute to the queue. Complete the following fields:

* **Attribute Name**:
    Enter the name of the attribute.
* **Content Type**:
    Enter the content type to be used for sending the message to SQS (for example, `text/plain`).
* **Content Encoding**:
    Enter the content encoding.

### Advanced SQS settings

On the **Advanced**
tab you can configure how to handle messages that are larger than 256KB in size. Configure these fields:

**Split message into smaller ones**:
Select this option to split the message into smaller messages before sending it to the queue.

**Store in S3 and place pointer on SQS queue**:
Select this option to store the payload in Amazon S3 and place a pointer to S3 in the queue. Configure the S3 settings as described in [S3 settings](#s3-settings).
