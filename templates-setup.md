# CF QuickStart Setup

In order for the  *Launch Stack* buttons to work the CloudFormation templates must be hosted on an S3 bucket on AWS. This guide explains how to upload the templates to S3 using the aws cli.

1. Create an S3 Bucket to store the CloudFormation templates. The bucket must be publicy accessible.

2. Package and upload the templates to S3:

    ```console
    $ aws cloudformation package \
        --template-file master.yml \
        --output-template-file master.packaged.yml \
        --s3-bucket <YOUR_S3_BUCKET>
    ```

    This command will replace the local references in the templates and upload them to S3. The output of this command is a template named *master.packaged.yml*.

3. Upload the *master.packaged.yml* file to S3:

    ```console
    $ aws s3 cp master.packaged.yml s3://<YOUR_S3_BUCKET>/
    ```

Once the templates are uploaded in S3 you can copy the URL of the *master.packaged.yml* file and use it in the *Launch Stack* buttons URL.

[![launch_stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=casper-0&templateURL=https://nclouds-cloudformation-templates.s3-us-west-2.amazonaws.com/casper/master.packaged.yml&region=us-west-2) 

Note that the URL of the button is a CloudFormation URL with some query string parameters to pre-populate the CloudFormation fields.

### Supporting multiple regions

CloudFormation requires that the S3 bucket that hosts the templates must be in the same region of the Stack that will be deployed, so multiple buckets will be required to support multiple regions.

There are two options to achieve this:

1. Create multiple buckets and upload the templates manually to each bucket.
2. Create multiple buckets and enable S3 replication.

The easiest way to maintain multiple buckets is choosing a primary bucket and enable S3 replication to replicate the changes to each of the buckets on the different regions. For more information read the [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html)

nClouds recommends to support just a few popular regions for the quickstart templates and provide the manual deployment option for customers that intend to use a not supported region.