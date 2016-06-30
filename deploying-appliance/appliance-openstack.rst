Installing the Virtual Appliance in OpenStack
**************************************************

ElasticBox can run as a virtual appliance in your OpenStack private cloud in Grizzly or Havana. Here are the requirements, high-level steps, and a `video`_ that shows how to install the appliance.

**In this article:**

* `OpenStack requirements`_
* `Installing the appliance in OpenStack`_
* `Next steps`_

OpenStack requirements
-------------------------

ElasticBox requires the following OpenStack services:

* Identity Service (Keystone)
* Compute (Nova)
* Image (Glance)
* Networking (Nova-net or Neutron). This is optional.
* Block storage (Cinder). Optional, unless you want to add volume storage to the ElasticBox appliance VM.

.. _video:

Installing the Appliance in OpenStack
----------------------------------------

Follow the video for detailed help.

.. raw:: html

	<div class="doc-image padding-1x">
		<iframe src="//player.vimeo.com/video/121204949" width="561" height="316" frameborder="0" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe>
	</div>

**Steps**

1. Create a flavor with these attributes:
	* 2 VCPUs
	* 2048 MB RAM
	* 100 GB root disk

2. Create an image of the appliance. Under **Image Location**, paste the appliance image download link you got from `support`_. The appliance image is in QCOW2 compressed format.
3. Launch an instance of ElasticBox from the appliance image and flavor.

.. _support: support@elasticbox.com

Next Steps
-------------

* `Configure networking </../documentation/deploying-appliance/appliance-networking/>`_
* `Set up the appliance for use </../documentation/deploying-appliance/appliance-initialsetup/>`_

