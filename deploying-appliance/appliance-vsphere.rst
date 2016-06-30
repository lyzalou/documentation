Installing the Virtual Appliance in vCenter
************************************************

If you don't have internet access in the vCenter, download all the files and import them locally into your vCenter network. If you have access, you can directly import the appliance in the open virtualization format (OVF) and vCenter will download the disk images for you.

**In this article:**

* `vCenter requirements`_
* `Installing the appliance in vCenter`_
* `Connecting the appliance to the vCenter network`_
* `Next steps`_

vCenter Requirements
------------------------

* Datacenter running vSphere vCenter 5.0 or later
* Linux virtual machine template
* 4 CPU cores, 8 GB memory, 100 GB hard drive space

Installing the Appliance in vCenter
---------------------------------------

**Steps**

1. Copy the OVF file download URL.
	**Note: If you don’t have internet access, download all the files and import them locally into your vCenter network.**
2. In the vCenter vSphere client, click **File** > **Deploy OVF Template**.
3. In the wizard, under Deploy from a file or URL, paste in the URL. Click **Next**.
4. (Optional) Edit the name and select the folder where the appliance will live. Click **Next**.
5. Select the cluster and a host that will run the appliance. Click **Next**.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/appliance/appliance-vsphere-selectclusterandhost.png" alt="Select a Cluster and Host">
		</div>

6. Choose a cluster resource pool if available to centrally manage the compute and storage resources for the appliance.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/appliance/appliance-vsphere-chooseclusterresourcepool.png" alt="Choose a Cluster Resource Pool">
		</div>

7. Select the datastore to store the appliance. The datastore virtual disk format is set to Thin. Thin provision conserves disk space and expands as needed. This means the appliance uses the space required now but takes up more space as usage grows.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/appliance/appliance-vsphere-selectdatastore.png" alt="Select a Datastore">
		</div>

8. Review the appliance settings before you deploy. Select **Power on after deployment**. Click **Finish**. 

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/appliance/appliance-vsphere-reviewovfsettings.png" alt="Review the Appliance OVF Settings">
		</div>

Connecting the Appliance to the vCenter Network
-------------------------------------------------

In these steps, you connect the ElasticBox appliance to the vCenter datacenter network and then turn it on.

**Steps**

1. In the vSphere Client, right-click the ElasticBox appliance that you just installed.
2. Click **Edit Settings** > **Hardware** tab. The OVF template pre-selects the required processing speed and the network adapter. Make sure they are set as follows:

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/appliance/appliance-vsphere-reviewingprocessingspeedandnetworkadaptersettings.png" alt="Check the CPU Speed and Network Adapter Settings">
		</div>

	* CPUs. Two CPU virtual sockets to increase parallel processing.
	* Network adapter. A valid network to connect the appliance to your datacenter.

3. Now start the appliance. Right-click the appliance, click **Power** > **Power On**.

Next Steps
-------------

* `Configure networking </../documentation/deploying-appliance/appliance-networking/>`_
* `Set up the appliance for use </../documentation/deploying-appliance/appliance-initialsetup/>`_

