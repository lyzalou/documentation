Tips on Troubleshooting ElasticBox
*************************************

Check the solutions to common issues before you contact `ElasticBox support </../documentation/troubleshooting/contacting-support/>`_ in the case of a problem.

**In this article**

* `Instance unreachable during an operation`_
* `Instance is stuck deploying`_
* `Instance is still terminating`_
* `Instance hangs at the install ElasticBox agent step`_
* `Quick start box not installing properly`_
* `ElasticBox appliance is unavailable`_

Instance unreachable during an operation
-------------------------------------------

**Reason**

When you trigger a lifecycle operation on an instance, it goes into a processing state because the agent is not running at all or not running properly. This problem can affect instances launched through the ElasticBox web interface, the API, or directly using the agent command.

**Solution**

Install the agent on the instance to bring it online. Then re-run the lifecycle operation.

1. Connect to the instance by SSH or RDP.

2. Install the agent. The command uses the token of the older agent to connect to the instance.

	Linux instances deployed from the ElasticBox cloud service:

	.. raw:: html

		<pre>
		curl -ks ebx.co | sudo bash
		</pre>

	Windows instances deployed from the ElasticBox cloud service (run the command as a PowerShell administrator):

	.. raw:: html

		<pre>
		(New-Object Net.WebClient).DownloadString("http://ebx.co") | iex
		</pre>

	Linux instances deployed from the ElasticBox Appliance:

	.. raw:: html

		<pre>
		curl -ks 10.0.0.1 | sudo bash
		</pre>

	Windows instances deployed from the ElasticBox Appliance (run the command as a PowerShell administrator):

	.. raw:: html

		<pre>
		(New-Object Net.WebClient).DownloadString("http://10.0.0.1") | iex
		</pre>

	**Note**: Replace 10.0.0.1 with your appliance IP or hostname.

Instance is stuck deploying
------------------------------

**Reason 1**

.. _Restart the agent on the Linux or Windows instance:

An instance can't update its state because the ElasticBox agent can't connect to ElasticBox. Poor agent network connectivity can cause this issue.

**Solution**

1. Restart the agent on the Linux or Windows instance:

	Linux:

	.. raw:: html

		<pre>
		sudo /usr/elasticbox/elasticbox restart
		</pre>

	Windows (run in command prompt):

	.. raw:: html

		<pre>
		net stop elasticbox
		net start elasticbox
		</pre>

2. In ElasticBox, open the lifecycle editor of the instance and click **Reinstall**. The instance should reflect the proper status.

**Reason 2**

The Linux or Windows image on the instance doesn't have cloud-init.

**Solution**

ElasticBox needs cloud-init for its agent to execute box script configuration on the VM image. Install cloud-init on the image and launch a new instance.

**Reason 3**

When box scripts exit with 100 typically because of apt-get failures, the ElasticBox agent goes into sleep mode waiting for the machine or service to reboot.

**Solution**

* Place the apt-get exit code in the file executing it and to the last line of executable code, add this:

	.. raw:: html

		<pre>
		[[ "$?" -eq "100" ]] && exit 1
		</pre>

* `Restart the agent on the Linux or Windows instance`_.

Instance is still terminating
--------------------------------

**Reason**

Several concurrent terminate requests within seconds of each other can cause a race condition that keeps the instance in flux.

**Solution**

Force the instance to terminate. In the lifecycle editor of the instance, click **Force Terminate**.

Instance hangs at the install ElasticBox agent step
------------------------------------------------------

**Reason**

Something causes the agent to hang even though it's running on the instance.

**Solution**

1. Log in to the instance and kill the agent.
2. Redeploy the instance from the lifecycle editor in ElasticBox. The agent should start deploying.

Quick start box not installing properly
-----------------------------------------

**Reason**

A broken puppet manifest or chef recipe can cause the box to not install properly.

**Solution**

`Contact support </../documentation/troubleshooting/contacting-support/>`_ to alert us of the broken box. In the meantime, you can define the install in a box using bash commands like apt-get, wget, cURL, and more.

ElasticBox appliance is unavailable
---------------------------------------

**Reason**

When you configure the hostname, SSL certificate, or block device for the appliance, there's a chance that the appliance may become unavailable. This issue can happen because you set the hostname incorrectly, upload a non-matching SSL certificate and key, or the block device gets corrupt, runs out of space or dismounts from the system. As a result, the appliance fails to reboot. Instance operations fail, and the appliance can't reach deployed instances properly.

**Solution**

`Contact support </../documentation/troubleshooting/contacting-support/>`_. We will walk you through recovering the appliance. Send us the appliance version number shown at the top of the `setup console </../documentation/deploying-appliance/appliance-initialsetup/>`_. And send us the logs you can download from the Logs section of the appliance setup console.

Logs help us debug your appliance issues. The .log files include recent audit information like who did what, user connections, and contain the logs of all the services.







