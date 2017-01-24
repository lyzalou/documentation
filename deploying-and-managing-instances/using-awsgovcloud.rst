Using AWS GovCloud
******************

AWS GovCloud is an isolated region designed to handle workloads of sensitive data. Why spin up machines and SSH into them to run workloads manually, when you can automate and deploy faster using `ElasticBox </../documentation/getting-started/what-does-elasticbox-do/>`_?

AWS GovCloud is available only by request. Contact `support`_ to enable it for your organization.

.. _support: support@elasticbox.com

**In this article:**

* `Connect to AWS GovCloud from ElasticBox`_
* `Add Custom AMIs </../documentation/deploying-and-managing-instances/using-your-aws-account/#add-customamis>`_
* `Deploy to AWS GovCloud from ElasticBox`_

Connect to AWS GovCloud from ElasticBox
---------------------------------------

Access to AWS GovCloud requires a `special signup process <http://docs.aws.amazon.com/govcloud-us/latest/UserGuide/getting-set-up.html>`_. When you have a GovCloud account, connect to it in ElasticBox with a role based ARN following the same steps as you would for a regular AWS account.

`Step 1. Delegate a role for ElasticBox in your AWS GovCloud account to allow third-party API access </../documentation/deploying-and-managing-instances/using-your-aws-account/#awscrossaccount-role>`_.

Enter the following 3rd party IAM information for ElasticBox:

* Account ID: 948203093918
* External ID: ebx
* Require MFA: Leave unselected

`Step 2. Share the role's Amazon Resource Name (ARN) in ElasticBox. </../documentation/deploying-and-managing-instances/using-your-aws-account/#awsshare-roleARN>`_.

Deploy to AWS GovCloud from ElasticBox
--------------------------------------

Select the box where you configured a workload to run remotely and choose AWS deployment settings to launch in a VM or service in the GovCloud region.

ElasticBox provisions machines in EC2 or a VPC with deployment settings you pick. These settings include AMI, instance type, key pairs, availability zone, security groups, Elastic IP, block storage, load balancing, and auto scaling. After provisioning the VM or service, ElasticBox deploys the box scripts to install, configure, and start the workload.

Deploy to any of the services in the AWS GovCloud region.

* `EC2 (Linux and Windows) </../../documentation/deploying-and-managing-instances/using-your-aws-account/#aws-ec2>`_
* `AWS RDS </../documentation/deploying-and-managing-instances/using-your-aws-account/#aws-rds>`_
* `AWS S3 </../documentation/deploying-and-managing-instances/using-your-aws-account/#aws-s3>`_
* `AWS DynamoDB </../documentation/deploying-and-managing-instances/using-your-aws-account/#aws-dynamodb>`_
* `AWS CloudFormation </../documentation/configuring-and-managing-boxes/template-box/#cloudformation-box/>`_
