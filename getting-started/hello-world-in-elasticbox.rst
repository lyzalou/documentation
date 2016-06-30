Hello World in ElasticBox
********************************

Learn how ElasticBox works in minutes with Hello World.

Configure a box with reusable, infrastructure-independent configuration that you can deploy to any public or private cloud provider. What’s more, you can collaborate with your team to manage the deployments.

Before you start, `sign up for an ElasticBox account <https://elasticbox.com/signup>`_. It’s free!

Then, follow these simple steps.

* `Configure a Hello World box`_
* `Add a provider and deployment policy`_
* `Deploy the box`_
* `Collaborate with team members`_

Configure a Hello World box
----------------------------------

On the Boxes page, click **New** > **Script** box. Under Requirements, select Linux to indicate that the box needs a Linux OS to deploy and save.

.. raw:: html

	<div class="doc-image padding-1x">
      <img class="img-responsive" src="/../assets/img/docs/tutorials/helloworld-createnewbox.png" alt="Create a New Box Based on Linux Compute">
    </div>

Add a greeting variable
===========================

Configure the box to install a greeting in the virtual environment. To store your Hello World greeting, under Configuration > Variables, create a text variable called **Greeting** and enter **Hello World** as its value and save.

**Note:** Add a port variable with a value of 22 if you plan to SSH into the Hello World instance after deploying.

.. raw:: html

	<div class="doc-image padding-1x">
      <img class="img-responsive" src="/../assets/img/docs/tutorials/helloworld-addgreetingvariable.png" alt="Add a Text Variable Called Greeting">
    </div>

Automate the box install
===========================

To install the greeting on the instance, add an install event. Under Configuration > Events > install, click **install**.

.. raw:: html

	<div class="doc-image padding-1x">
      <img class="img-responsive" src="/../assets/img/docs/tutorials/event-install.png" alt="Add an Event to Automate Box Install">
    </div>

In the event editor, paste this bash script to call the Greeting variable:

.. raw:: html

	<pre>
  #!/bin/bash
  echo "${Greeting}" > /tmp/hello
	</pre>

When you deploy this box, the event creates a file with the greeting in the tmp folder on a newly provisioned instance in the virtual environment.

Add a provider and deployment policy
-----------------------------------------

You’ve configured the Hello World box. To deploy, you first need to connect to a cloud provider like `AWS </../documentation/deploying-and-managing-instances/using-your-aws-account/>`_, `Google Cloud </../documentation/deploying-and-managing-instances/using-your-google-cloud-account/>`_, `vSphere </../documentation/deploying-and-managing-instances/using-the-vsphere-private-datacenter/>`_, `OpenStack </../documentation/deploying-and-managing-instances/using-the-openstack-cloud/>`_, `CloudStack </../documentation/deploying-and-managing-instances/using-cloudstack/>`_, or `Azure </../documentation/deploying-and-managing-instances/using-azure/>`_ to launch the box using their services. For this walkthrough, we'll use AWS.

Add a provider
===========================

On the Providers page, click **New Provider** to add your AWS account.

* If you're on ElasticBox in the cloud, enter `the ElasticBox role ARN </../documentation/deploying-and-managing-instances/using-your-aws-account/#connect-awsaccount>`_ from your AWS account.
* If you're on the ElasticBox appliance, enter the `AWS root account key ID and secret credentials <https://console.aws.amazon.com/iam/home?#security_credential>`_ or create an IAM user and enter those credentials.

Once you connect to AWS, all the default AMIs, VPCs, security groups, key pairs, and available services in your account are automatically accessible from ElasticBox.

.. raw:: html

	<div class="doc-image padding-1x">
      <img class="img-responsive" src="/../assets/img/docs/providers/aws-credentials-enter-rolearn.png" alt="Add a Provider">
    </div>

Create a deployment policy
==============================

Next, create a deployment policy to select the services and infrastructure resources from the provider. In the Boxes page, click **New** > **Deployment Policy** box.

.. raw:: html

	<div class="doc-image padding-1x">
      <img class="img-responsive" src="/../assets/img/docs/tutorials/helloworld-adddeploymentpolicy.png" alt="Add a Deployment Policy">
    </div>

* Under Provider, select the AWS account you added.
* Under Claims, select Linux as the service the policy provides to deployments. Then save.

Deploy the Box
-------------------

On the Boxes page, hover on the hello world box and click the play button to deploy.

.. raw:: html

	<div class="doc-image padding-1x">
      <img class="img-responsive" src="/../assets/img/docs/tutorials/helloworld-createnewinstance.png" alt="Create a New Instance of the Hello World Box">
    </div>

* Under Deployment Policy, select the policy box you created.
* Optionally, tag the instance with useful information like dev or test.
* Under Expiration, set the instance to automatically terminate in an hour.
	**Note**: When you deploy to any cloud, the cloud provider may charge you. So it's a good idea to `auto schedule the instance </../documentation/deploying-and-managing-instances/deploying-managing-instances/#instance-scheduler>`_ to terminate right after this tutorial to avoid costs.
* Under Variables, you can enter a different greeting before you deploy.

You're all set to to launch the hello world box in AWS. Click **Deploy**.

Collaborate with Team Members
------------------------------

You just configured and deployed a simple hello world app in minutes to the cloud. Did you know that you can share with other ElasticBox users and work together on deployments? Let’s share the Hello World instance with your team members.

On the Instances page, click the Hello World instance. Click **Share**. Add your team members and give them view or edit access. They can now access this instance from their personal workspaces.

.. raw:: html

	<div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/instances/instance-hello-world-share.png" alt="Collaborate with Others">
          </div>
      </div>
    </div>

Congrats, you successfully shared an automated configuration with others and can now collaborate to build an even better one!
