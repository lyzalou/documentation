Deployment Policy Boxes
********************************

Give access to cloud provider infrastructure using a deployment policy box. Policies help allocate cloud resources securely rather than giving access to the entire cloud provider. As IT operations, you have control over what and how much resources deployments consume. Customize policies to support specific deployment scenarios. For example, you may want to provide a small instance type of a certain Linux distribution in a policy to launch development environments.

**In this article:**

* `Create a deployment policy`_
* `Give access to the policy`_
* `Control box deployments with admin boxes`_

Create a Deployment Policy
-------------------------------

On the Boxes page, click **New** > **Deployment Policy**. Here select a cloud provider added in ElasticBox and optionally add `claim tags </../documentation/core-concepts/boxes/#box-metadata>`_. Save to continue.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/boxes/policybox-createnew.png" alt="Create a New Deployment Policy Box">
	</div>

Click **Edit** to customize the policy. Select the resource, network, and other deployment settings from the cloud provider. See the provider help for details.

* `Google Cloud </../documentation/deploying-and-managing-instances/using-your-google-cloud-account/#deploy-in-account>`_

* `Amazon Web Services </../documentation/deploying-and-managing-instances/using-your-aws-account/#deploying-to-aws>`_

* `AWS GovCloud </../documentation/deploying-and-managing-instances/using-awsgovcloud/#govcloud-deploy>`_

* `VMware vCenter </../documentation/deploying-and-managing-instances/using-the-vsphere-private-datacenter/#deploying-to-vsphere>`_

* `Azure </../documentation/deploying-and-managing-instances/using-azure/#deploy-in-azure>`_

* `OpenStack </../documentation/deploying-and-managing-instances/using-the-openstack-cloud/#deploy-openstack>`_

* `Rackspace Cloud </../documentation/deploying-and-managing-instances/using-rackspacecloud/#rackspace-deploy>`_

* `CloudStack </../documentation/deploying-and-managing-instances/using-cloudstack/#cloudstack-deploy-eb>`_

* `SoftLayer </../documentation/deploying-and-managing-instances/using-softlayer/#softlayer-deploy>`_

Give Access to the Policy
-------------------------------

Once you set up the policy, give team workspaces and individuals access to cloud resources for their box deployments. Click **Share** in the deployment policy box to give view, edit, or owner access.

* Owner. Rename or delete the policy metadata and edit the policy settings if you have edit access to the provider registered in ElasticBox.

* Edit. Change the policy box metadata and edit the policy settings if you have edit access to the provider.

* View. Consume the policy to deploy boxes.

Control Box Deployments with Admin Boxes
------------------------------------------

Any script box attached to a deployment policy is an admin box. The admin box allows enterprise IT operations teams to run common admin tasks in deployments to comply with company policies and best practices. Such common admin tasks can include installing monitoring agents, registering virtual machines in a database, or setting up public keys on all machines before making them available to users.

Admin box use cases
```````````````````````

Admin boxes are useful in these deployment scenarios:

* **Install a monitoring agent**. An admin box can locally install a monitoring agent like Nagios or New Relic that can monitor virtual machine activities and send data back to a central monitoring service.

* **Set the hostname**. An admin box can set the hostname of every virtual machine deployed to a provider’s environment.

* **Register virtual machines on a server**. For every virtual machine deployed, an admin box, for example, can register it to a Chef Master server and then release it when the machine terminates.

* **Install certificates**. An admin can install certificates locally on every virtual machine in production as a simple example.

Creating and executing an admin box
``````````````````````````````````````

To create an admin box, open a deployment policy and add a script box under Variables. Typically, you want to add a script box that matches the policy OS type. In a Windows policy for example, add a script box that runs on Windows. A policy can have as many admin boxes as needed.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/boxes/policybox-addadminboxes.png" alt="Add Admin Boxes to a Deployment Policy">
	</div>

When a box launches on a deployment policy containing an admin box, ElasticBox wraps it like a child box in the admin box deployment. In each main event type such as install, configure, start, stop, the admin box runs first followed by events of the box launched. To execute admin box events before others within each event sub category like pre-install, install, move the commands to the admin box pre-install, pre-configure, and pre-start events.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/boxes/policybox-adminbox-orderscriptsexecute.png" alt="Order in which Admin Box and Script Box Scripts Execute">
	</div>
