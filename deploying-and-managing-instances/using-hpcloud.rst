Using HP Cloud
********************************

Use ElasticBox when you deploy to HP Helion Public Cloud to automate and deploy applications remotely and rapidly.

Manage the lifecycle of both the VM and the applications via ElasticBox. ElasticBox provisions Linux virtual machines based on the regional network, security group, and floating IP settings you select. Then ElasitcBox installs application workloads that you model in boxes onto the VMs. We manage your application workloads and provision, orchestrate deployments on Linux servers in HP Cloud through the open source OpenStack Nova Python client.

**In this article:**

* `Connect HP Cloud in ElasticBox`_
* `Deploy to HP Cloud from ElasticBox`_

Connect HP Cloud in ElasticBox
---------------------------------------

**Prerequisites**

You need the following to deploy in HP Cloud:

* Get an account in `HP Helion Public Cloud <https://horizon.hpcloud.com>`_.
* Enable HP Cloud as a cloud provider in the `Admin Console </../documentation/managing-your-organization/provider-access/>`_. HP Cloud is only available in the ElasticBox Enterprise Edition.
* Activate Compute service for the HP Cloud account in the regions you want to deploy. We deploy to both US East (region-b-geo-1) and US West (region-a.geo-1) in the availability zones within each.
* Activate Object Storage service in the region you're deploying if you want to add volumes to instances.

Follow these steps to connect a HP Cloud account in ElasticBox.

**Steps**

1. In ElasticBox, on the Providers page, click **New Provider**.

2. Select **HP Cloud** and enter the credentials as shown and save.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/providers/hpcloud-connect-provider.png" alt="Enter HP Cloud Account Credentials">
		</div>

	* **Identity URL**. Identity endpoint for the US West or US East region, whichever is closest to you.
	* **Project**. Project name that is visible at the top left corner of the HP Cloud Console.
	* **Username**. Username you use to login to HP Cloud Console.
	* **Password**. Password you use to login to the HP Cloud Console.

Deploy to HP Cloud from ElasticBox
-------------------------------------

Choose from these deployments options to `launch application workloads </../documentation/deploying-and-managing-instances/deploying-managing-instances/>`_ to Linux servers.

To apply tags from ElasticBox on to instances deployed in HP Cloud, see `Tag Instances </../documentation/managing-your-organization/resource-tags/>`_.

**Deployment**

+----------------------------------+------------------------------------------------+
| Option                           | Description                                    |
+==================================+================================================+
| Provider                         | Select an HP Cloud account connected in        |
|                                  | ElasticBox.                                    |
+----------------------------------+------------------------------------------------+

**Resource**

+----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
| Option                           | Description                                                                                                              |
+==================================+==========================================================================================================================+
| Location                         | Select the US West (region-a.geo-1) or US East (region-b-geo-1) region to deploy.                                        |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
| Availability Zone                | Choose an availability zone in the region: az1, az2, or az3. HP Cloud recommends that you deploy to one or more          |
|                                  | availability zones to increase fault tolerance for critical applications.                                                |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
| Image                            | Select a Linux OS image from your HP Cloud account. You can select a public or custom image depending upon your          |
|                                  | deployment type.                                                                                                         |
|                                  |                                                                                                                          |
|                                  | To deploy to a custom image, add it to the HP Cloud account first, and then sync the account in ElasticBox. Be sure that |
|                                  | the custom image has cloud-init. The startup script is required to run the ElasticBox agent that executes box scripts on |
|                                  | the VM. You don't need to worry about public images because they inherently run cloud-init.                              |
|                                  |                                                                                                                          |
|                                  | We've tested that the following Linux distributions deploy using ElasticBox successfully: Debian 7.x, Centos 6.3, Centos |
|                                  | 7, Ubuntu 12.04, Ubuntu 14.04, Fedora 21.                                                                                |
|                                  |                                                                                                                          |
|                                  | **Note**: Only deployments on Linux servers are supported because HP Cloud does not yet officially support               |
|                                  | `cloud-init on Windows images <http://docs.hpcloud.com/publiccloud/api/compute/>`_.                                      |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
| Flavor                           | Select an image size that dictates the number of VCPUs, the root disk, ephemeral disk, and RAM that the virtual machine  |
|                                  | uses. For information on flavors, see the `HP Cloud docs <http://docs.hpcloud.com/publiccloud/api/compute>`_.            |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
| Key Pair                         | If you uploaded a key pair to the HP Cloud account, select it to SSH into the instance you launch. Make sure you add or  |
|                                  | import key pairs in the HP Cloud account first and then sync your account in ElasticBox to select it from here.          |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
| Instances                        | Specify the number of instances to provision.                                                                            |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------+

**Volumes**

Increase the instance storage and add better I/O performance for your applications by adding volumes. We attach and mount them to the instances through the OpenStack block storage service API.

**Note**: To take snapshots or backup volumes, you have to handle those tasks directly in HP Cloud.

* Add volumes as an image and one or more hard volumes by specifying their size.
* An **Image Volume** lets you create a copy of the selected image as a volume and boot the instance from it. Check **Persist** to retain the disk and data after you terminate an instance. If you don't persist the storage, it is ephemeral.
* A hard volume lets you add disks for extra storage. We attach and mount the disks to an instance when it goes live and detach and persist them after you terminate the instance.

**Network**

+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| Option                           | Description                                                                                                                                |
+==================================+============================================================================================================================================+
| Network                          | Select a network for the region. If you create additional networks for the region in HP Cloud, they appear in this drop-down.              |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| Security Groups                  | Select **Automatic** to let ElasticBox launch the VM in a new security group. Or select a default or existing security group in the region.|
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| Floating IP Pool                 | To allow fixed, external access via SSH or RDP to the VM, assign the VM a floating IP. But first request a floating IP pool from the HP    |
|                                  | Cloud account. When deploying, you can assign one from the pool to the VM from this drop-down.                                             |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+

