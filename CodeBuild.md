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
- The maximum size of a file that you can upload by using the Amazon S3 console is 78 GB
- Amazon S3 creates buckets in the AWS Region that you specify. You can choose any AWS Region that 
is geographically close to you to optimize latency, minimize costs, or address regulatory requirements. 
For example, if you reside in Europe, you might find it advantageous to create buckets in the EU (Ireland) 
or EU (Frankfurt) regions.
- Understanding more on bucket restrictions at:
http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html
 ```
 ## Step 2: Create Source Code to Build
Open Java compatible text editor (i.e Eclipse) and create the following directory structure.
 ```
 --src
      |--main
          |--java
      |--test
          |--java
 ```
 
Next create the following file in src/main/java

 MessageUtil.java and implement the following code:
 
 ```
 public class MessageUtil {
  private String message;
 
  /* Constructor sets the strings of characters
  public MessageUtil(String message) {
    this.message = message;
  }
  /* This method creates the output
  public String printMessage() {
    System.out.println(message);
    return message;
  }
  /* Produce the output of Hi and a string of characters casted into the message variable
  public String salutationMessage() {
    message = "Hi!" + message;
    System.out.println(message);
    return message;
  }
}
 ```
 ![img4](https://cloud.githubusercontent.com/assets/25268970/25297238/8176bb96-26ba-11e7-9d5f-ae49d9d8de45.jpg)
 
 Next create the following file in src/test/jave: 
 
 TestMessageUtil.java and implement the following code:
 
 ```
package test;

import org.junit.Test;
import main.java.MessageUtil;
import org.junit.Ignore;
import static org.junit.Assert.assertEquals;

public class TestMessageUtil {
	String message = "Robert";    
	  MessageUtil messageUtil = new MessageUtil(message);
	   
	  @Test
	  public void testPrintMessage() {      
	    System.out.println("Inside testPrintMessage()");     
	    assertEquals(message,messageUtil.printMessage());
	  }

	  @Test
	  public void testSalutationMessage() {
	    System.out.println("Inside testSalutationMessage()");
	    message = "Hi!" + "Robert";
	    assertEquals(message,messageUtil.salutationMessage());
	  }
}
 ```
 Next create the pom.xml file in the root folder where the src folder is stored and implement the following code:
 
 ```
 <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>org.example</groupId>
	<artifactId>messageUtil</artifactId>
	<version>1.0</version>
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
</project>
 ```
 ![img6](https://cloud.githubusercontent.com/assets/25268970/25298714/a31e5654-26c5-11e7-9857-dfa0b98a9f8c.jpg)

```
Apache Maven will use the instructions in this file to convert the MessageUtil.java and TestMessageUtil.java files into a file named messageUtil-1.0.jar and then run the specified tests.
```
## Step 3: Create Build Spec
```
Quick Note: A build spec is a collection of build commands and related settings, in YAML format, that AWS CodeBuild uses to run a build. Without a build spec, AWS CodeBuild will not be able to successfully convert your build input into build output, nor will it be able to locate the build output artifact in the build environment to upload to your output bucket.
```
Create in buildspec.yml file in the root folder
```
version: 0.1

phases:
  install:
    commands:
      - echo Nothing to do in the install phase...
  pre_build:
    commands:
      - echo Nothing to do in the pre_build phase...
  build:
    commands:
      - echo Build started on `date`
      - mvn install
  post_build:
    commands:
      - echo Build completed on `date`
artifacts:
  files:
    - target/messageUtil-1.0.jar
```
## Step 4: Add the Source Code and the Build Spec to the Input Bucket
Navigate to the local folder storing all the files that were just built and zip all files stored in the directory. Do not include the (root directory name) directory.

Then navigate to https://console.aws.amazon.com/s3/ in your internet browser and double-click the input bucket.

Upload the MessageUtil.zip file to the input bucket.

![img7](https://cloud.githubusercontent.com/assets/25268970/25300149/e78fb196-26d6-11e7-9f43-c1fafdf09dd6.jpg)

As displayed above, a window will populate with a set of processess that allow the user to change settings and permissions to the files that are being uploaded. The user can bypass this process by directly selecting and upload button and this will automatically set default values for the processes.

## Step 5 Create the Build Project
```
Quick Note: A build project defines how AWS CodeBuild will run a build. It includes information 
such as where to get the source code, the build environment to use, the build commands to run, 
and where to store the build output. A build environment represents a combination of operating 
ystem, programming language runtime, and tools that AWS CodeBuild uses to run a build. For this 
build environment, you'll instruct AWS CodeBuild to use a Docker image that contains a version 
of the Java Development Kit (JDK) and Apache Maven.
```

Navigate to https://us-west-2.console.aws.amazon.com/codebuild/home?region=us-west-2#/introduction

Ensure that the correct region is selected. This can be found next to the User Account Name.


## Resource
https://aws.amazon![img4](https://cloud.githubusercontent.com/assets/25268970/25297238/8176bb96-26ba-11e7-9d5f-ae49d9d8de45.jpg).com/s3/faqs/

http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html

http://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html

http://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html

https://docs.docker.com/engine/docker-overview/

