- The AWS Free Tier includes 100 build minutes of build.general1.small per month with <b>AWS CodeBuild</b>.
- AWS Free Tier includes 5GB storage, 20,000 Get Requests, and 2,000 Put Requests with <b>Amazon S3</b>.
- 20,000 requests/month with <b>AWS Key Management Service</b>
- AWS Free Tier includes 10 Metrics, 10 Alarms, and 1,000,000 API requests with <b>Amazon Cloudwatch</b>.

## Step 1: Creating/Configuring an S3 Bucket
Create two S3 buckets.
- One of these buckets will store the build input. 
Name this input bucket codebuild-region-ID-account-ID-input-bucket, where region-ID 
represents the AWS region of the bucket, and account-ID represents your AWS account ID.

- The other bucket will store the build output. Name this output bucket codebuild-region-ID-account-ID-output-bucket.

<i>Quick notes: 
- Amazon S3 is cloud storage for the Internet to store data (photos, videos, documents etc.)
- You can use buckets to group related objects in the same way that you use a directory to group files in a file system.
- There is no limit to the number of objects that can be stored in a bucket and no difference in performance whether you use many buckets
or just a few. You can store all of your objects in a single bucket, or you can organize them across several buckets.
- Amazon S3 creates buckets in the AWS Region that you specify. You can choose any AWS Region that is geographically close to you to optimize latency, minimize costs, or address regulatory requirements. For example, if you reside in Europe, you might find it advantageous to create buckets in the EU (Ireland) or EU (Frankfurt) regions.
 </i>

## Resource
https://aws.amazon.com/s3/faqs/
