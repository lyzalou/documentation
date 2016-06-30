Deploying and Managing Instances
**************************************

**In this article:**

* `Deploying a new instance`_
* `Creating a deployment profile`_
* `Scheduling Instances`_
* `Handling instance lifecycle states`_

Deploying a New Instance
--------------------------------

An instance is an instantiated version of a box launched to provider's virtual infrastructure or your own. Follow these steps to launch one.

**Steps**

1. Click **Instances** > **New Instance**.

2. Select a box. You can search and look through the tabs.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/instances/instance-selectboxfromcatalog.png" alt="Select a Box from the Catalog">
		</div>

	* **My Boxes**. Shows boxes you created in your workspace.
	* **Shared with Me**. Shows boxes others shared with you.
	* **Quick Starts**. Shows default boxes available to all ElasticBox users. These include service boxes such as Linux Compute, Windows Compute; includes AWS services like MySQL Database, Oracle Database, DynamoDB, PostgreSQL Database, and S3 bucket; and includes the Azure Microsoft SQL Database service. You can directly launch an instance to these database services. While you can’t modify those boxes, you can combine them with other boxes to build multi-tiered applications.

	**Note**: Don't find a box you’re looking for? Check if you’re in the right workspace. Remember that you may not have access if the box is no longer shared with you.

3. In the New Instance dialog, specify the environment name and deployment profile.

	* **Environment**. Give a name to recognize the instance.
	* **Deployment Profile**. Select a previously created deployment profile or create a new one. For details, see `Creating a Deployment Profile`_.

4. In the New Instance dialog, pass deployment parameters under **Variables**. Before launching, you can override and provide fresh values.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/instances/instance-provideconfigurationvalues.png" alt="Provide Configuration Values through Variables">
		</div>

	* Listed here are all the variables defined in the main box as well as those within nested boxes or box type variables.
	* Required variables are marked with an asterisk. To see all variables including optional ones, click **Show More**.
	* When a variable is required, you must specify its value to launch an instance of the box. If optional, you can launch without giving values, and do it later in the `lifecycle editor </../documentation/core-concepts/lifecycle-editor/>`_.
	* `Binding type variables </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-bindingtype>`_ are also listed here. Depending on how it’s defined in the box, you can select as its value any instance or that of a specific box type deploying or active in the workspace.

Creating a Deployment Profile
--------------------------------

A deployment profile defines settings for your infrastructure that are applied at deployment time. These settings include the cloud provider that will host the deployed boxes, how the virtual infrastructure will be sized, and where it will be placed.

**Steps**

1. In the New Instance dialog, click **New Profile**.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/instances/instance-createnewdeploymentprofile.png" alt="Create a New Deployment Profile">
		</div>

2. Enter a profile name. Typically deployment profiles are named for the stage in the application lifecycle they represent, such as Dev, Test, QA, Pre-Production, and Production. However you could also use them to represent geographical or logical environments.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/instances/instance-createnewdeploymentprofile-name.png" alt="Enter a Profile Name">
		</div>

3. Click **Create**.

4. Configure the deployment profile settings for the box you selected from the catalog.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/instances/instance-configuredeploymentsettings.png" alt="Configure Deployment Profile Settings">
		</div>

Settings in the deployment profile vary by the provider you deploy to. For provider specific deployment settings, see these articles:

* `Using Google Cloud </../documentation/deploying-and-managing-instances/using-your-google-cloud-account/>`_
* `Using Your AWS Account </../documentation/deploying-and-managing-instances/using-your-aws-account/>`_
* `Using the vSphere Private Datacenter </../documentation/deploying-and-managing-instances/using-the-vsphere-private-datacenter/>`_
* `Using Your OpenStack Cloud </../documentation/deploying-and-managing-instances/using-the-openstack-cloud/>`_
* `Using CloudStack </../documentation/deploying-and-managing-instances/using-cloudstack/>`_
* `Using Azure </../documentation/deploying-and-managing-instances/using-azure/>`_

**Note**: If you’re deploying to public cloud providers like AWS or Google Compute, you’ll most likely be charged by the cloud provider for the virtual infrastructure you provision. Familiarize yourself with their pricing model as ElasticBox assumes no responsibility for costs incurred.

Scheduling Instances
--------------------------------

Save on compute and hosting costs by scheduling instances at launch time. Rather than remember to turn off a machine manually, schedule it to stop automatically at your convenience. When launching, you can schedule an instance to shut down or terminate at a given UTC time.

We notify you of instances about to expire in 24 hours by email at around 12 AM UTC. From the email, you can navigate to the instance page and change the schedule if you like. If you don't get this email, check your email spam filters or check if `SMTP outbound is on </../documentation/deploying-appliance/appliance-initialsetup/#appliance-initial-outboundemail>`_ in the setup console for the ElasticBox appliance.

Follow these steps to schedule an instance.

**Steps**

1. From the Instances page, click **New Instance**.

2. Select a box you want to deploy.

3. In the New Instance dialog, select the **Shutdown** or **Terminate** operation from the Expiration drop-down.

	Select **Always on** if you don't want to schedule anything. Shutdown powers off the instance while Terminate deletes the instance on the provider's side.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/instances/schedule-instance-chooseoperation.png" alt="Select Operation to Schedule on Instance">
		</div>

4. For the selected operation, set a predefined or custom UTC schedule.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/instances/schedule-instance-selectschedule.png" alt="Select Instance Schedule">
		</div>

5. When done, click **Deploy**.

**Note**: Even if you don't schedule an instance at the time of deploying, you can do so later. Once online, you can go to an instance page and in **Edit Details**, set the schedule.

Besides the user interface, you can automatically schedule instances using the instances API with a `POST </../documentation/api/instances/#post-instances>`_ or `PUT </../documentation/api/instances/#put-instances-instance_id>`_ request.

Handling Instance Lifecycle States
----------------------------------------

Instance actions (on the instances page or the lifecycle editor) trigger deployment related event scripts from your box. Take these actions to start, stop, terminate an instance, and even perform upgrades or make changes to your live instance.

Some actions are available only after the instance changes state. For example, you can’t forcibly terminate an instance until you’ve terminated it first.

Go to the Admin Console to `manage several instances </../documentation/managing-your-organization/manage-assets-monitor-usage/#admin-manage-instances>`_ spread across users and workspaces in your organization.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/instances/instance-states.png" alt="Instance Actions">
	</div>

**Reconfigure**

This executes the configure events from the box.

**Reinstall**

This re-runs the install scripts from your boxes onto the existing virtual infrastructure. This is useful if you made changes to your scripts within this instance, say to upgrade the instance to a new box version. A reconfigure automatically follows the reinstall.

**Power On**

This virtually powers on your instance. It’s useful in case you’ve shut the instance down. After powering on, the configure and start scripts from the box execute.

**Shut Down**

This runs the stop scripts from your box instance and cleanly shuts down the OS. It’s useful if your instance does not need to be up 24/7. As some cloud providers only charge for running instances, this can save money.

**Terminate**

This executes the dispose scripts from your box instance and then deletes the virtual infrastructure. You can’t revert the action and since you can lose data, be sure that you want to perform this action in the first place.

**Force Terminate**

If a Terminate fails for some reason (maybe a broken dispose script) then this forcibly deletes the virtual infrastructure. If you previously terminated or deleted an instance from the provider's side, the instance may linger in Force Terminate in ElasticBox. Give it a couple of minutes then try to force-terminate again.

**Delete**

Click the delete icon after you Terminate or Force Terminate an instance. Until then, the box instance page and logs are retained in the Elasticbox database. But delete completely removes the box instance page.
