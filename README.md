# Voice Powered Analytics

### Introduction
In this workshop, you will build an Alexa skill that queries metrics from a data lake, which you will define.  The goal after leaving this workshop, is for you to understand how to uncover Key Performance Indicators (KPIs) from a data set, build and automate queries for measuring those KPIs, and access them via Alexa voice-enabled devices.  Startups can make  available voice powered analytics to query at any time, and Enterprises can deliver these types of solutions to stakeholders so they can have easy access to the Business KPIs that are top of mind.         
This workshop requires fundamental knowledge of AWS services, but is designed for first time users of QuickSight, Athena, and Alexa. We have broken the workshop into three sections or focus topics. 
These are:

* Data Discovery using QuickSight
* Building Data Lake analytics in Athena (based on objects in S3) to generate answers for Alexa
* Building a custom Alexa skill to access the analytics queries from Athena

We expect most attendees to be able to complete the full workshop in **2 hours**. 

To help keep moving through the sections in case you get stuck anywhere, we have provided CloudFormation templates and sample code.

For those feeling creative, many sections also have **Bonus Sections** where you can build additional capability on top of the workshop.  
Feel free to engage your workshop facilitator(s)/lab assistant(s) if you'd like additional assistance with these areas.  

You can also contact @chadneal or @WestrichAdam on twitter if you have additional questions or feedback.

## Prerequisites

Please make sure you have the following available prior to the workshop.

* <a href="https://developer.amazon.com/alexa" target="_blank">Amazon Developer</a> account (Free) **Note** This is different from a typical AWS workflow. 
* AWS Account with admin or full access to all services
* Using QuickSight requires <a href="http://docs.aws.amazon.com/quicksight/latest/user/sign-up-existing.html" target="_blank">Signing up for QuickSight</a>

## Lab Setup

We have provided a CloudFormation template to create baseline resources needed by this lab but are not the focus of the workshop. These include IAM Roles, IAM Policies, a DynamoDB table, and a CloudWatch Event rule. These are listed as outputs in the CloudFormation template in case you want to inspect them.

**Please launch the template below so that the resources created will be ready by the time you get to those sections in the lab guides.** 

**Pick the desired region that's closest to your location for optimal performance **

When you launch the template you will be asked for a few inputs. Use the following table for reference. 

Input Name | Value
:---: | :---:
Stack Name | VPA-Setup
DDBReadCapacityUnits | 5
DDBWriteCapacityUnits | 5

<details>
<summary><strong>Watch a video of launching CloudFormation (Click to expand)</strong></summary><p>

![launcg CloudFormation](https://github.com/awslabs/voice-powered-analytics/blob/master/media/images/vpa-cloudformation-launch.gif)

</details>

<table><tr><td>Region</td> <td>Launch Template</td></tr>
<tr>
<td>EU-WEST-1</td> <td><a href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=VPA-Setup&templateURL=https://s3.amazonaws.com/aws-vpa-tweets-euw1/setup/vpa_setup.yaml" target="_blank"><IMG SRC="/media/images/CFN_Image_01.png"></a></td></tr> <tr><td>US-EAST-1</td> <td><a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=VPA-Setup&templateURL=https://s3.amazonaws.com/aws-vpa-tweets/setup/vpa_setup.yaml" target="_blank"><IMG SRC="/media/images/CFN_Image_01.png"></a></td></tr></table>


## Modules



By default, you can access twitter data that exists in a public S3 bucket filtered on #reinvent, #aws or @AWSCloud. If you'd like to use this pre-existing data, you can skip to Module 1.   But if you'd like to deploy this workshop through building your own Data Lake and using your own filters, follow the outlined steps below:
<details>
<summary>Optional Module 0 (Build Your Own Data Lake)</summary>

### Step 1: Generate Twitter Keys
1.  Go to http://twitter.com/oauth_clients/new
2. Click "Create New App"
3. Under Name, enter something descriptive, e.g., aws-voice-twitter-data lake
4. Enter a description
5. Under Website, you can enter the website of your choosing
6. Leave Callback URL blank
7. Read and agree to the Twitter Developer Agreement
8. Click "Create your Twitter application"

### Step 2: Create Kinesis Firehose Instance to send data to S3
9.  In the console, navigate to **Services > Kinesis**
10.  On the left pane, click **Data Firehose**, then click **Create Delivery Stream**
11.  Under *Delivery Stream Name*, type a name and log this name to a location that you'll use in Step 3.
12. Leave default and click **Next**, click **Next** on the next screen.
13.  For *Select Destination*, choose **Amazon S3**
14.  You can either create a new bucket and prefix or reference an existing one
15.  Leave defaults on *Configure Settings* except for *IAM Role*.  **Note: Ensure that the firehose IAM role has a policy to *write* to the bucket specified in the previous section

Once your stream is created, navigate to the next series of steps to deploy the Application using the Twitter tokens and Firehose delivery stream

### Step 3: Deploy App In Repo

16.  Navigate to [Twitter-Poller to-Kinesis-Firehose](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:381943442500:applications~Twitter-Poller-to-Kinesis-Firehose) in the Serverless Application Repository.
17.  Click the **Deploy** button (top righthand corner)
18.  You may be prompted to login to your AWS account.  After doing so, scroll down to the *Configure Application Parameters* section where you will be able to enter:
 a. The 4 Tokens received from Twitter
 b. The Kinesis Firehose name that the serverless solution will send the data to (as created in Step 2)
 c. Customize the search text that twitter will bring back
19.  After deploying the Serverless Application, your application will begin polling automatically within the next 5 minutes.
</details>

#### Main Modules:
1. [Amazon QuickSight Section](README-QuickSight.md) 
1. [Amazon Athena Section](README-Athena.md)
1. [Amazon Alexa Section](README-Alexav2.md)
1. (Optional) [Alexa For Business Section](README-A4B.md) - Deploy your skill privately through Alexa For Business

After you have completed the workshop you can disable to the CloudWatch Event to disable the Athena poller. This will stop the automated scans of s3 from Athena and also serve to stop any further Athena costs. If you want to completely remove all resources please follow the [cleanup guide](README-Cleanup.md).

