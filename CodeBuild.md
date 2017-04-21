- The AWS Free Tier includes 100 build minutes of build.general1.small per month with <b>AWS CodeBuild</b>.
- AWS Free Tier includes 5GB storage, 20,000 Get Requests, and 2,000 Put Requests with <b>Amazon S3</b>.
- 20,000 requests/month with <b>AWS Key Management Service</b>
- AWS Free Tier includes 10 Metrics, 10 Alarms, and 1,000,000 API requests with <b>Amazon Cloudwatch</b>.

## Step 1: Creating/Configuring an S3 Bucket
Create two S3 buckets by navigating to the AWS Management Console and opening the Amazon S3 Console. 
Quck Access Link: https://console.aws.amazon.com/s3/home?region=us-west-2

![img1](https://cloud.githubusercontent.com/assets/25268970/25292857/0c9c757c-26a6-11e7-9dff-cc3b7f05a5fa.jpg)

- One of these buckets will store the build input. 
Name this input bucket codebuild-region-ID-account-ID-input-bucket, where region-ID 
represents the AWS region of the bucket, and account-ID represents your AWS account ID. Select Next.

![im2](https://cloud.githubusercontent.com/assets/25268970/25293147/7bc3910a-26a7-11e7-9abe-4f29a1909ee2.jpg)

A set properties window will populate. No properties will be set at this time, select Next.

![image](https://cloud.githubusercontent.com/assets/25268970/25293246/f668ab0c-26a7-11e7-8789-b7577ee79e9e.png)

Next, the Permissions Settings page will populate. No permissions need to be set at this time, select Next. Then review the report of the settings and select Create Bucket to complete the process.

![image](https://cloud.githubusercontent.com/assets/25268970/25293516/449d5754-26a9-11e7-886d-3eaacaa9f51c.png)
```
Notes: 
Versioning – Versioning enables you to keep multiple versions of an object in one bucket. 
Versioning is disabled for a new bucket by default. For information on enabling versioning, 
see How Do I Enable or Suspend Versioning for an S3 Bucket?.

Logging – Server access logging provides detailed records for the requests made to your 
bucket. By default, Amazon S3 does not collect server access logs. To enable logging for the bucket, 
choose Logging. To disable logging, choose Disable logging. Choose Save to save your settings.

Tags – With AWS cost allocation, you can use tags to annotate billing for your use of a bucket. 
A tag is a key-value pair that represents a label that you assign to a bucket. To add tags, choose Tags, 
and then choose Add tag.
```
- The other bucket will store the build output. Name this output bucket codebuild-region-ID-account-ID-output-bucket.

![img3](https://cloud.githubusercontent.com/assets/25268970/25293739/30bc17f6-26aa-11e7-98b7-9e2d2ae39a83.jpg)

```Quick notes: 
- Amazon S3 is cloud storage for the Internet to store data (photos, videos, documents etc.)
- You can use buckets to group related objects in the same way that you use a directory to group 
files in a file system.
- There is no limit to the number of objects that can be stored in a bucket and no difference in 
performance whether you use many bucketsor just a few. You can store all of your objects in a single 
bucket, or you can organize them across several buckets.
- Amazon S3 creates buckets in the AWS Region that you specify. You can choose any AWS Region that 
is geographically close to you to optimize latency, minimize costs, or address regulatory requirements. 
For example, if you reside in Europe, you might find it advantageous to create buckets in the EU (Ireland) 
or EU (Frankfurt) regions.
- Understanding more on bucket restrictions at:
http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html
 ```
 
## Resource
https://aws.amazon.com/s3/faqs/
http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html

