## ARJ-Stack: AWS Cloud Development Kit (AWS CDK) - Setup

- [What is AWS CDK?](#what-is-aws-cdk)
- [Setup: CDK with Python (On Windows)](#setup-cdk-with-python-on-windows)
    - [Install AWS CLI](#install-aws-cli)
	- [Configure Workstation with AWS Credentials and an AWS Region](#configure-workstation-with-aws-credentials-and-an-aws-region)
	- [Install Python](#install-python)
	- [Install Python Package Installer (pip) and Virtual Environment Manager (virtualenv)](#install-python-package-installer-pip-and-virtual-environment-manager-virtualenv)
	- [Install NodeJS](#install-nodejs)
	- [Install AWS CDK](#install-aws-cdk)
	- [Bootstrapping](#bootstrapping)
	- [Bootstrapping Process](#bootstrapping-process)
		- [AWS CloudFormation Template](#aws-cloudformation-template)
		- [AWS CDK ToolKit](#aws-cdk-toolkit)
- [Resources to Provision in Bootstrapping](#resources-to-provision-in-bootstrapping)
- [Bootstrapping Customization](#bootstrapping-customization)
- [Test the CDK Setup](#test-the-cdk-setup)
	- [App Initialization](#app-initialization)
	- [Python Virual Environment Activation](#python-virual-environment-activation)
	- [Dependencies Installation](#dependencies-installation)
	- [Verifying the Stacks](#verifying-the-stacks)
	- [Synthesize Stack](#synthesize-stack)
	- [Stack Deployment](#stack-deployment)
	- [Stack Deletion](#stack-deletion)

### What is AWS CDK?
- AWS Cloud Development Kit is a native framework designed by AWS for defining AWS Cloud Infrastructure as code in one of the support programming language and to provision it using AWS CloudFormation.

- It comes with a support to different languages: TypeScript, JavaScript, Python, Java, C# and GO. It has a become a atttraction for the people coming from programming background and want to define/provision/manage cloud resource with comfort. 

- Each CDK app can contain multiple stacks which gets converted into CLoudFormation Stack at runtime. Each such stack can contain multiple construct where each construct represents one of AWS Cloud resource such as Lambda Function, S3 Bucket etc.

- Well, there are 3 stages for this to happen. 
    - Build: Identify the syntactical and Type Error in the code (as with any programmign language).
    - Synthesis: Identify the logical errors in your infrastructure code constructs for defining AWS resources.
    - Deployment: This is an action stage so identify all the permission issues, and if all well, it simply provision the resources.

### Setup: CDK with Python (On Windows)

There are few steps that we need to follow to setup/configure the environment:

#### Install AWS CLI
---

***Reference:*** https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html</i>

Run the following command in the command prompt window:

```
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

It will popup the installation instruction dialog as mentioned below. Follow the instruction to get it done.

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/aws-cli2-installation-started.jpg">

One the installation is completed. you will see the folllowing message in the dialog window.

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/aws-cli2-installation-completed.jpg">

To confirm the installation. Open a new command prompt window and run the command `python --version`. You will get the output as mentioned below:

```
D:\aws-cdk>aws --version
aws-cli/2.9.21 Python/3.9.11 Windows/10 exe/AMD64 prompt/off
```

#### Configure Workstation with AWS Credentials and an AWS Region
---

Run the following command `aws configure`

```
D:\aws-cdk>aws configure --profile aws-cdk-setup
AWS Access Key ID [None]: <Your Access Key ID>
AWS Secret Access Key [None]: <Your Secret Access key>
Default region name [None]: ap-south-1
Default output format [None]: json

D:\aws-cdk>
```
- You can use any AWS profile name and AWS region code. I have used `aws-cdk-setup` as AWS profile name and `ap-south-1` as AWS region.

The above command will create 2 files:

`~/.aws/config or %USERPROFILE%\.aws\config`

This file will have the following entry:

> [profile aws-cdk-setup]<br>
> region = ap-south-1<br>
> output = json<br>

`~/.aws/credentials or %USERPROFILE%\.aws\credentials`

This file will have the following entry:

> [aws-cdk-setup]<br>
> aws_access_key_id = \<Your Access Key ID\><br>
> aws_secret_access_key = \<Your Secret Access key\><br>

#### Install Python 
---

AWS CDK requires Python 3.7 or later. We are installing Python 3.11.1 (Latest version as at the time of writing this page..)

- You can find the latest release of Python from its download page [https://www.python.org/downloads/](https://www.python.org/downloads/)

Run the following command in the command prompt window:

```
start /i python-3.11.1-amd64.exe
```

It will popup the installation instruction dialog as mentioned below. Follow the instruction to get it done.

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/python-installation.jpg">

To confirm the installation. Open a new command prompt window and run the command `aws --version`. You will get the output as mentioned below:

```
D:\aws-cdk>python --version
Python 3.11.1

D:\aws-cdk>
```

#### Install Python Package Installer (pip) and Virtual Environment Manager (virtualenv)
---

AWS CDK also requires pip and virtualenv to be installed. Run the following commands to get it done.

1. Bootstrap the pip installer into an existing Python installation or virtual environment.

```
D:\aws-cdk>python -m ensurepip --upgrade
Looking in links: c:\Users\ADMIN\AppData\Local\Temp\tmp1c5r9mo_
Requirement already satisfied: setuptools in c:\users\admin\appdata\local\programs\python\python311\lib\site-packages (65.5.0)
Requirement already satisfied: pip in c:\users\admin\appdata\local\programs\python\python311\lib\site-packages (22.3.1)
```

2. Install/Upgrade pip using module pip

```
D:\aws-cdk>python -m pip install --upgrade pip
Requirement already satisfied: pip in c:\users\admin\appdata\local\programs\python\python311\lib\site-packages (22.3.1)
Collecting pip
  Using cached pip-23.0-py3-none-any.whl (2.1 MB)
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 22.3.1
    Uninstalling pip-22.3.1:
      Successfully uninstalled pip-22.3.1
Successfully installed pip-23.0
```

3. Install/Upgrade virtualenv using module pip

```
D:\aws-cdk>python -m pip install --upgrade virtualenv
Collecting virtualenv
  Using cached virtualenv-20.17.1-py3-none-any.whl (8.8 MB)
Collecting distlib<1,>=0.3.6
  Using cached distlib-0.3.6-py2.py3-none-any.whl (468 kB)
Collecting filelock<4,>=3.4.1
  Using cached filelock-3.9.0-py3-none-any.whl (9.7 kB)
Collecting platformdirs<3,>=2.4
  Downloading platformdirs-2.6.2-py3-none-any.whl (14 kB)
Installing collected packages: distlib, platformdirs, filelock, virtualenv
Successfully installed distlib-0.3.6 filelock-3.9.0 platformdirs-2.6.2 virtualenv-20.17.1
```

#### Install NodeJS
---

Irrespective of the CDK programming language used (even with Java, C# or Python), all the CDK app use the same backend, which runs on Node.js, hence we must install Node.js 10.13.0 or later, on the workstation.

> Node.js versions 13.0.0 through 13.6.0 are not compatible with the AWS CDK due to compatibility issues with its dependencies

We are installing node-v19.6.0-x64 (Latest version as at the time of writing this page..)

- You can find the latest release of Node.js from its download page [https://nodejs.org/en/download/current/](https://nodejs.org/en/download/current/)

Run the following command in the command prompt window:

```
msiexec.exe /i ./node-v19.6.0-x64.msi
```

It will popup the installation instruction dialog as mentioned below. Follow the instruction to get it done.

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/node-installation-started.jpg">

One the installation is completed. you will see the folllowing message in the dialog window.

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/node-installation-completed.jpg">

To confirm the installation. Open a new command prompt window and run the command `node --version` and `npm --version`. You will get the output as mentioned below:

```
D:\aws-cdk>node --version
v19.6.0

D:\aws-cdk>npm --version
9.4.1

D:\aws-cdk>
```

#### Install AWS CDK
---

We will install AWS CDK toolkit with global scope using the Node Package Manager. Run the following command in command prompt window:

```
D:\aws-cdk>npm install -g aws-cdk

changed 1 package in 4s

D:\aws-cdk>
```

We can verify whether installation is correct and which version of AWS CDK got installed by running the following command:

```
D:\aws-cdk>cdk --version
2.63.2 (build e08e34a)

D:\aws-cdk>
```

#### Bootstrapping
---

When we define the resources in the AWS CDK, it is converted into CloudFormation Stack and the generated CloudFormation stack then gets deployed. AWS CloudFormation, for being able to deploy these stacks, need S3 buckets for storing files and IAM roles that grant permissions needed to perform deployment. The process of provisioning these prequisite resources for the AWS CDK iteself is called Bootstrapping.

AWS already provides a CloudFormation template for creating this bootstrap stack, usually named as CDKToolkit.

You can find this template using the following ways:

> GitHub<br>

[https://github.com/aws/aws-cdk/blob/main/packages/aws-cdk/lib/api/bootstrap/bootstrap-template.yaml](https://github.com/aws/aws-cdk/blob/main/packages/aws-cdk/lib/api/bootstrap/bootstrap-template.yaml)

> Through CDK Command line utility

```
cdk bootstrap --show-template > bootstrap-template.yaml
```

#### Bootstrapping Process
---
We can perform bootstrapping in 2 different ways:

##### AWS CloudFormation Template

By now, we already know how to get access to the cloudformation template for bootstrapping so just get it and run the following command:

```
D:\aws-cdk>aws cloudformation create-stack ^
More?   --stack-name CDKToolkit ^
More?   --template-body file://bootstrap-template.yaml ^
More?   --capabilities CAPABILITY_NAMED_IAM ^
More?   --profile aws-cdk-setup
{
    "StackId": "arn:aws:cloudformation:ap-south-1:<Your Account ID>:stack/CDKToolkit/37672d90-a532-11ed-877b-06b4a2344da0"
}

D:\aws-cdk>
```

The above command will result in a CloudFormation Stack (`CDKToolkit`) deployed which you can see in CloudFormation Dashboard.

##### AWS CDK ToolKit

We can use the `cdk bootstrap` command to bootstrap the CDK toolkit in your AWS environment (which is combination of AWS Account and AWS Region).

```
D:\aws-cdk>cdk bootstrap aws://<AWS Account No>/ap-south-1 --profile aws-cdk-setup
 ⏳  Bootstrapping environment aws://<AWS Account No>/ap-south-1...
Trusted accounts for deployment: (none)
Trusted accounts for lookup: (none)
Using default execution policy of 'arn:aws:iam::aws:policy/AdministratorAccess'. Pass '--cloudformation-execution-policies' to customize.
CDKToolkit: creating CloudFormation changeset...
CDKToolkit |  0/12 | 2:43:36 PM | REVIEW_IN_PROGRESS   | AWS::CloudFormation::Stack | CDKToolkit User Initiated
CDKToolkit |  0/12 | 2:43:42 PM | CREATE_IN_PROGRESS   | AWS::CloudFormation::Stack | CDKToolkit User Initiated
CDKToolkit |  0/12 | 2:43:46 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | ImagePublishingRole
CDKToolkit |  0/12 | 2:43:46 PM | CREATE_IN_PROGRESS   | AWS::S3::Bucket         | StagingBucket
CDKToolkit |  0/12 | 2:43:46 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | CloudFormationExecutionRole
CDKToolkit |  0/12 | 2:43:46 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | LookupRole
CDKToolkit |  0/12 | 2:43:46 PM | CREATE_IN_PROGRESS   | AWS::SSM::Parameter     | CdkBootstrapVersion
CDKToolkit |  0/12 | 2:43:46 PM | CREATE_IN_PROGRESS   | AWS::ECR::Repository    | ContainerAssetsRepository
CDKToolkit |  0/12 | 2:43:46 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | FilePublishingRole
CDKToolkit |  0/12 | 2:43:47 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | ImagePublishingRole Resource creation Initiated
CDKToolkit |  0/12 | 2:43:47 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | LookupRole Resource creation Initiated
CDKToolkit |  0/12 | 2:43:47 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | FilePublishingRole Resource creation Initiated
CDKToolkit |  0/12 | 2:43:47 PM | CREATE_IN_PROGRESS   | AWS::SSM::Parameter     | CdkBootstrapVersion Resource creation Initiated
CDKToolkit |  0/12 | 2:43:48 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | CloudFormationExecutionRole Resource creation Initiated
CDKToolkit |  0/12 | 2:43:48 PM | CREATE_IN_PROGRESS   | AWS::S3::Bucket         | StagingBucket Resource creation Initiated
CDKToolkit |  1/12 | 2:43:48 PM | CREATE_COMPLETE      | AWS::SSM::Parameter     | CdkBootstrapVersion
CDKToolkit |  1/12 | 2:43:48 PM | CREATE_IN_PROGRESS   | AWS::ECR::Repository    | ContainerAssetsRepository Resource creation Initiated
CDKToolkit |  2/12 | 2:43:49 PM | CREATE_COMPLETE      | AWS::ECR::Repository    | ContainerAssetsRepository
CDKToolkit |  3/12 | 2:44:10 PM | CREATE_COMPLETE      | AWS::S3::Bucket         | StagingBucket
CDKToolkit |  3/12 | 2:44:11 PM | CREATE_IN_PROGRESS   | AWS::S3::BucketPolicy   | StagingBucketPolicy
CDKToolkit |  3/12 | 2:44:12 PM | CREATE_IN_PROGRESS   | AWS::S3::BucketPolicy   | StagingBucketPolicy Resource creation Initiated
CDKToolkit |  4/12 | 2:44:13 PM | CREATE_COMPLETE      | AWS::S3::BucketPolicy   | StagingBucketPolicy
CDKToolkit |  5/12 | 2:44:21 PM | CREATE_COMPLETE      | AWS::IAM::Role          | ImagePublishingRole
CDKToolkit |  6/12 | 2:44:21 PM | CREATE_COMPLETE      | AWS::IAM::Role          | FilePublishingRole
CDKToolkit |  7/12 | 2:44:22 PM | CREATE_COMPLETE      | AWS::IAM::Role          | CloudFormationExecutionRole
CDKToolkit |  8/12 | 2:44:22 PM | CREATE_COMPLETE      | AWS::IAM::Role          | LookupRole
CDKToolkit |  8/12 | 2:44:23 PM | CREATE_IN_PROGRESS   | AWS::IAM::Policy        | FilePublishingRoleDefaultPolicy
CDKToolkit |  8/12 | 2:44:24 PM | CREATE_IN_PROGRESS   | AWS::IAM::Policy        | FilePublishingRoleDefaultPolicy Resource creation Initiated
CDKToolkit |  8/12 | 2:44:25 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | DeploymentActionRole
CDKToolkit |  8/12 | 2:44:26 PM | CREATE_IN_PROGRESS   | AWS::IAM::Role          | DeploymentActionRole Resource creation Initiated
CDKToolkit |  8/12 | 2:44:38 PM | CREATE_IN_PROGRESS   | AWS::IAM::Policy        | ImagePublishingRoleDefaultPolicy
CDKToolkit |  8/12 | 2:44:39 PM | CREATE_IN_PROGRESS   | AWS::IAM::Policy        | ImagePublishingRoleDefaultPolicy Resource creation Initiated
CDKToolkit |  9/12 | 2:44:57 PM | CREATE_COMPLETE      | AWS::IAM::Policy        | FilePublishingRoleDefaultPolicy
CDKToolkit | 10/12 | 2:45:01 PM | CREATE_COMPLETE      | AWS::IAM::Role          | DeploymentActionRole
CDKToolkit | 11/12 | 2:45:12 PM | CREATE_COMPLETE      | AWS::IAM::Policy        | ImagePublishingRoleDefaultPolicy
CDKToolkit | 12/12 | 2:45:14 PM | CREATE_COMPLETE      | AWS::CloudFormation::Stack | CDKToolkit
 ✅  Environment aws://<AWS Account No>/ap-south-1 bootstrapped.

D:\aws-cdk>

```

- You can get the AWS account number either through AWS console or using the following command in Command Prompt window:

```
D:\aws-cdk>aws sts get-caller-identity --profile aws-cdk-setup
{
    "UserId": "<AWS User ID>",
    "Account": "<AWS account number>",
    "Arn": "arn:aws:iam::<AWS account number>:user/<AWS User Name>"
}

D:\aws-cdk>
```

The value of the `"Account"` field in the output JSON is your AWS account number.

- You can get the AWS region code either through the following command in Command Prompt window:

```
D:\aws-cdk>aws configure get region --profile aws-cdk-setup
ap-south-1

D:\aws-cdk>
```

### Resources to Provision in Bootstrapping

The following resources are provisioned as part of the CDKToolkit CloudFormation template:

Qualifier: `hnb659fds` [An identifier to distinguish multiple bootstrap stacks in the same environment]

| Sr. No. | Resource Type | Resource Name | Additional Details |
|:------|:------|:------|:------|
| 1 | `AWS::CloudFormation::Stack` | `CDKToolkit` |  |
| 2 | `AWS::SSM::Parameter` | `/cdk-bootstrap/hnb659fds/version` | `CdkBootstrapVersion` |
| 3 | `AWS::IAM::Role` | `cdk-hnb659fds-image-publishing-role-<AWS Account Number>-ap-south-1` | `ImagePublishingRole`: Trusted Account Role |
| 4 | `AWS::IAM::Policy` |  | `ImagePublishingRoleDefaultPolicy`: Policy attacched to `ImagePublishingRole` |
| 5 | `AWS::IAM::Role` | `cdk-hnb659fds-file-publishing-role-<AWS Account Number>-ap-south-1` | `FilePublishingRole`: Trusted Account Role |
| 6 | `AWS::IAM::Policy` |  | `FilePublishingRoleDefaultPolicy`: Policy attacched to `FilePublishingRole` |
| 7 | `AWS::IAM::Role` | `cdk-hnb659fds-cfn-exec-role-<AWS Account Number>-ap-south-1` | `CloudFormationExecutionRole`: AWS Service (CloudFormation) Account Role with Policy `AdministratorAccess` attached |
| 8 | `AWS::IAM::Role` | `cdk-hnb659fds-lookup-role-<AWS Account Number>-ap-south-1` | `LookupRole`: Trusted Account Role |
| 9 | `AWS::IAM::Role` | `cdk-hnb659fds-deploy-role-<AWS Account Number>-ap-south-1` | `DeploymentActionRole`: Trusted Account Role |
| 10 | `AWS::ECR::Repository` | `cdk-hnb659fds-container-assets-<AWS Account Number>-ap-south-1` | `ContainerAssetsRepository`: The ECR repository which hosts docker image assets |
| 11 | `AWS::S3::Bucket` | `cdk-hnb659fds-assets-<AWS Account Number>-ap-south-1` | `StagingBucket`: The S3 bucket used for file assets |
| 12 | `AWS::S3::BucketPolicy` |  | `StagingBucketPolicy`: Policy to be attached to the Bucket |
| 13 | `AWS::KMS::Key` |  | `FileAssetsBucketEncryptionKey`: KMS Key to encrypt the bucket contents |
| 14 | `AWS::KMS::Alias` |  | `FileAssetsBucketEncryptionKeyAlias`: KMS Key alias |


### Bootstrapping Customization

We can also customize the bootsrapping resources. 

- One way is to customize the CloudFormation template as per your need and either use `aws cloudformation` command to refer to customized template or use the following CDK command

```
cdk bootstrap --template custom-bootstrap-template.yaml
```

- The other way is when we dont have many customization; CDK command comes with some predefine options as defines below:

| Sr. No. | Command Option | Purpose |
|:------|:------|:------|
| 1 | `--bootstrap-bucket-name` | It overrides the name of the Amazon S3 bucket |
| 2 | `--bootstrap-kms-key-id` | It overrides the AWS KMS key used to encrypt the S3 bucket. |
| 3 | `--cloudformation-execution-policies` | This specifies the ARNs of managed policies that should be attached to the deployment role assumed by AWS CloudFormation during deployment of your stacks. |
| 4 | `--qualifier` | the string that is added to the names of all resources in the bootstrap stack.  |
| 5 | `--tags` | It adds one or more AWS CloudFormation tags to the bootstrap stack. |
| 6 | `--trust` | AWS accounts that may deploy into the environment being bootstrapped. |
| 7 | `--trust-for-lookup` | AWS accounts that may look up context information from the environment being bootstrapped. |
| 8 | `--termination-protection` | It prevents the bootstrap stack from being deleted. |

#### Example

The following command will override the default qualifier `hnb659fds` with `arjstack` and create the S3 bucket with name `arjstack-cdk`

```
cdk bootstrap aws://<AWS Account Number>/ap-south-1 ^
--qualifier arjstack ^
--bootstrap-bucket-name arjstack-cdk ^
--profile aws-cdk-setup
```

### Test the CDK Setup

#### App Initialization
---

At this moment, we can proceed building our first CDK project to test if our setup is working. Create a new directory named `aws-cdk-test` for the CDK Testing app. From within this directory, we need to run the cdk initialization using CDK project template `app`. Runt he following command: 

- Note: (Skipping the detailed output lines of this command intentionally to keep the documentation short)

```
D:\aws-cdk-test>cdk init app aws-cdk-test --language python
Applying project template app for python

# Welcome to your CDK Python project!

This is a blank project for CDK development with Python.

The `cdk.json` file tells the CDK Toolkit how to execute your app.

.....
......
........

Please run 'python -m venv .venv'!
Executing Creating virtualenv...
✅ All done!
```

After running this command, you will observe that `cdk init` command has generated a number of files and folders inside the app directory `aws-cdk-test`.

<br>Here is the screenshot of structure which is generated:

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/cdk-app-structure.jpg">

#### Python Virual Environment Activation
---

After init process is done, we need to activate the virtualenv. Run the following command:

```
D:\aws-cdk-test>.venv\Scripts\activate.bat

(.venv) D:\aws-cdk-test>
```

When you will run this command, you will notice that you are now inside python virtual environment. Its time for installing required dependencies so lets proceed with next step.

#### Dependencies Installation
---

Wehn you initialized the CDK app through CDK template, it created a `requirement.txt` file for you and pre-populated it with required dependencies such as the base aws-cdk package that has some initial functionality.

```
aws-cdk-lib==2.63.2
constructs>=10.0.0,<11.0.0
```

We can utilize this file to install these dependencies. Run the following command within your python virual environment:

- You can also add more dependencies in this file as per your need.

```
(.venv) D:\aws-cdk-test>python -m pip install -r requirements.txt
Collecting aws-cdk-lib==2.63.2
  Downloading aws_cdk_lib-2.63.2-py3-none-any.whl (26.5 MB)
     ---------------------------------------- 26.5/26.5 MB 4.7 MB/s eta 0:00:00
Collecting constructs<11.0.0,>=10.0.0
  Downloading constructs-10.1.242-py3-none-any.whl (57 kB)
     ---------------------------------------- 57.8/57.8 kB 3.2 MB/s eta 0:00:00
Collecting aws-cdk.asset-awscli-v1<3.0.0,>=2.2.52
  Downloading aws_cdk.asset_awscli_v1-2.2.59-py3-none-any.whl (13.4 MB)
     ------ --------------------------------- 2.1/13.4 MB 1.2 MB/s eta 0:00:10

....
......
....

Installing collected packages: publication, typing-extensions, typeguard, six, attrs, python-dateutil, cattrs, jsii, constructs, aws-cdk.asset-node-proxy-agent-v5, aws-cdk.asset-kubectl-v20, aws-cdk.asset-awscli-v1, aws-cdk-lib
Successfully installed attrs-22.2.0 aws-cdk-lib-2.63.2 aws-cdk.asset-awscli-v1-2.2.59 aws-cdk.asset-kubectl-v20-2.1.1 aws-cdk.asset-node-proxy-agent-v5-2.0.48 cattrs-22.2.0 constructs-10.1.242 jsii-1.74.0 publication-0.0.3 python-dateutil-2.8.2 six-1.16.0 typeguard-2.13.3 typing-extensions-4.4.0

[notice] A new release of pip available: 22.3.1 -> 23.0
[notice] To update, run: python.exe -m pip install --upgrade pip
```

#### Verifying the Stacks
---

You can verify if the CDK app is structured correctly by running the follwoing command:

```
(.venv) D:\aws-cdk-test>cdk ls --profile aws-cdk-setup
AwsCdkTestStack


(.venv) D:\aws-cdk-test>
```

You see that we have received the Stack name: `AwsCdkTestStack`

- We did not change anything about environment in `app.py` because we used the `--profile` option while we executed `cdk ls` command.

#### Synthesize Stack 
---

Lets change the application by introducing SQS Queue resource and sythensize it to see the AWS CloudFormation template.
<br>
Replace the file `aws_cdk_test\aws_cdk_test_stack.py` with the following contents:

```
from aws_cdk import (
    Duration,
    Stack,
    aws_sqs as sqs,
)
from constructs import Construct

class AwsCdkTestStack(Stack):

    def __init__(self, scope: Construct, construct_id: str, **kwargs) -> None:
        super().__init__(scope, construct_id, **kwargs)

        # SQS resource
        queue = sqs.Queue(
            self, "AwsCdkTestQueue",
            visibility_timeout=Duration.seconds(300),
        )
```

Now run the following command, and you will see the generated CloudFormation template as mentioned below.

- Note: This step is optional, `cdk deploy` step as mentioned the next, synthesizes your stack before each deployment.

```
(.venv) D:\aws-cdk-test>cdk synth --profile aws-cdk-setup
Resources:
  AwsCdkTestQueueFC7DA0D5:
    Type: AWS::SQS::Queue
    Properties:
      VisibilityTimeout: 300
    UpdateReplacePolicy: Delete
    DeletionPolicy: Delete
    Metadata:
      aws:cdk:path: AwsCdkTestStack/AwsCdkTestQueue/Resource
  CDKMetadata:
    Type: AWS::CDK::Metadata
    Properties:
      Analytics: v2:deflate64:H4sIAAAAAAAA/yXIyQmAMBBA0Vq8JyNRsAEbcClAYow4LhN0EkTE3t1O//ETyFJIIr2zNN0kZ2zhrL02k3hWwyvDWQYbrMh7+nC9qiy7sJnv5o469OjoEsXhB0dxCkqBikZGlFsgj4uF6u8NHMH2kG8AAAA=
    Metadata:
      aws:cdk:path: AwsCdkTestStack/CDKMetadata/Default
    Condition: CDKMetadataAvailable
Conditions:
  CDKMetadataAvailable:
    Fn::Or:
      - Fn::Or:
          - Fn::Equals:
              - Ref: AWS::Region
              - af-south-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-east-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-northeast-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-northeast-2
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-south-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-southeast-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-southeast-2
          - Fn::Equals:
              - Ref: AWS::Region
              - ca-central-1
          - Fn::Equals:
              - Ref: AWS::Region
              - cn-north-1
          - Fn::Equals:
              - Ref: AWS::Region
              - cn-northwest-1
      - Fn::Or:
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-central-1
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-north-1
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-south-1
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-west-1
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-west-2
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-west-3
          - Fn::Equals:
              - Ref: AWS::Region
              - me-south-1
          - Fn::Equals:
              - Ref: AWS::Region
              - sa-east-1
          - Fn::Equals:
              - Ref: AWS::Region
              - us-east-1
          - Fn::Equals:
              - Ref: AWS::Region
              - us-east-2
      - Fn::Or:
          - Fn::Equals:
              - Ref: AWS::Region
              - us-west-1
          - Fn::Equals:
              - Ref: AWS::Region
              - us-west-2
Parameters:
  BootstrapVersion:
    Type: AWS::SSM::Parameter::Value<String>
    Default: /cdk-bootstrap/hnb659fds/version
    Description: Version of the CDK Bootstrap resources in this environment, automatically retrieved from SSM Parameter Store. [cdk:skip]
Rules:
  CheckBootstrapVersion:
    Assertions:
      - Assert:
          Fn::Not:
            - Fn::Contains:
                - - "1"
                  - "2"
                  - "3"
                  - "4"
                  - "5"
                - Ref: BootstrapVersion
        AssertDescription: CDK bootstrap stack version 6 required. Please run 'cdk bootstrap' with a recent version of the CDK CLI.



(.venv) D:\aws-cdk-test>
```

#### Stack Deployment
---

We have already verified the configuration by sythensizing it so its time for deploying this stack which you can do by executing the following command:


```
(.venv) D:\aws-cdk-test>cdk deploy --profile aws-cdk-setup

✨  Synthesis time: 21.19s

AwsCdkTestStack: building assets...

[0%] start: Building b2bc83281ea14b073656cdf59b6e4c003798365b664f3042a57baf482b955a03:current_account-current_region
[100%] success: Built b2bc83281ea14b073656cdf59b6e4c003798365b664f3042a57baf482b955a03:current_account-current_region

AwsCdkTestStack: assets built

AwsCdkTestStack: deploying... [1/1]
[0%] start: Publishing b2bc83281ea14b073656cdf59b6e4c003798365b664f3042a57baf482b955a03:current_account-current_region
[100%] success: Published b2bc83281ea14b073656cdf59b6e4c003798365b664f3042a57baf482b955a03:current_account-current_region
AwsCdkTestStack: creating CloudFormation changeset...
AwsCdkTestStack | 0/3 | 8:28:34 AM | REVIEW_IN_PROGRESS   | AWS::CloudFormation::Stack | AwsCdkTestStack User Initiated
AwsCdkTestStack | 0/3 | 8:28:40 AM | CREATE_IN_PROGRESS   | AWS::CloudFormation::Stack | AwsCdkTestStack User Initiated
AwsCdkTestStack | 0/3 | 8:28:43 AM | CREATE_IN_PROGRESS   | AWS::SQS::Queue    | AwsCdkTestQueue (AwsCdkTestQueueFC7DA0D5)
AwsCdkTestStack | 0/3 | 8:28:43 AM | CREATE_IN_PROGRESS   | AWS::CDK::Metadata | CDKMetadata/Default (CDKMetadata)
AwsCdkTestStack | 0/3 | 8:28:44 AM | CREATE_IN_PROGRESS   | AWS::SQS::Queue    | AwsCdkTestQueue (AwsCdkTestQueueFC7DA0D5) Resource creation Initiated
AwsCdkTestStack | 0/3 | 8:28:44 AM | CREATE_IN_PROGRESS   | AWS::CDK::Metadata | CDKMetadata/Default (CDKMetadata) Resource creation Initiated
AwsCdkTestStack | 1/3 | 8:28:44 AM | CREATE_COMPLETE      | AWS::CDK::Metadata | CDKMetadata/Default (CDKMetadata)
1/3 Currently in progress: AwsCdkTestStack, AwsCdkTestQueueFC7DA0D5
AwsCdkTestStack | 2/3 | 8:29:55 AM | CREATE_COMPLETE      | AWS::SQS::Queue    | AwsCdkTestQueue (AwsCdkTestQueueFC7DA0D5)
AwsCdkTestStack | 3/3 | 8:29:56 AM | CREATE_COMPLETE      | AWS::CloudFormation::Stack | AwsCdkTestStack

 ✅  AwsCdkTestStack

✨  Deployment time: 87.01s

Stack ARN:
arn:aws:cloudformation:ap-south-1:<AWS Account Number>:stack/AwsCdkTestStack/25053af0-a5ca-11ed-b993-06ca1beddde6

✨  Total time: 108.2s



(.venv) D:\aws-cdk-test>
```

If you analyse the output as mentioned in the above highlights, you see it has performed 2 steps:
1. Synthesis which took 21.19 seconds
2. Deployment which took 87.01 seconds

High Level Outcomes which got reflected behind the scene:

1. The above command first created the CloudFormation Temaplte and stored it in CDK ToolKit - S3 bucket which we created in one of the setup steps mentioned above.

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/cdk-outcome-template-in-s3-bucket.jpg">

2. It created a CloudFormation Stack named `AwsCdkTestStack`

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/cdk-outcome-cfn-stack.jpg">

3. The Deployment of the CFN stack `AwsCdkTestStack` resulted into SQS Queue named `AwsCdkTestStack-AwsCdkTestQueueFC7DA0D5-FtrMrgQCMGwI`

<img src="https://github.com/ankit-jn/devops-aws-cdk-setup/blob/main/screenshots/cdk-outcome-sqs.jpg">

#### Stack Deletion
---

Once our testing is done, we can destroy our CDK stack by running the following command:

```
(.venv) D:\aws-cdk-test>cdk destroy --profile aws-cdk-setup
Are you sure you want to delete: AwsCdkTestStack (y/n)? y
AwsCdkTestStack: destroying... [1/1]

 ✅  AwsCdkTestStack: destroyed


(.venv) D:\aws-cdk-test>
```

### Authors

Module is maintained by [Ankit Jain](https://github.com/ankit-jn) with help from [these professional](https://github.com/ankit-jn/devops-aws-cdk-setup/graphs/contributors).
