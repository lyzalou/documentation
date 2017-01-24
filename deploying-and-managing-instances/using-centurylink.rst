Using CenturyLink Cloud
***********************

Automate application deployments through ElasticBox when you launch to Linux or Windows virtual servers in the CenturyLink Cloud public cloud. ElasticBox simplifies deployments with a dedicated focus on applications rather than infrastructure. See the `benefits and use cases </../documentation/>`_.

**In this article:**

* `Register CenturyLink Cloud provider in ElasticBox`_
* `Deploy to CenturyLink Cloud from ElasticBox`_
* `Auto-Discover Instances`_

Register CenturyLink Cloud Provider in ElasticBox
-------------------------------------------------

You need a `CenturyLink Cloud account <https://www.ctl.io>`_ to be able to deploy from ElasticBox. When you have an account, follow these steps to register it in ElasticBox to automate your deployments.

**Steps**

1. In ElasticBox, go to Providers > New Provider and select **CenturyLink**.

2. Enter the CenturyLink username and password as shown and save.

    .. raw:: html

        <div class="doc-image padding-1x">
            <img class="img-responsive" src="/../assets/img/docs/providers/centurylink-add-provider-credentials.png" alt="Enter CenturyLink Cloud Credentials">
        </div>

Deploy to CenturyLink Cloud from ElasticBox
-------------------------------------------

Select from the following deployment profile options to launch workloads on Linux or Windows machines.

Note a couple of things about instances you deploy on CenturyLink Cloud through ElasticBox.

* Instance name. Each instance is assigned a name that has the format of DatacenterFirst_six_letters_of_instance_nameCounter. i.e.: UC1CITTPUBLIC01 for an instance deployed on UC1 (US West - Santa Clara) of a box called Public Proxy.
* Instance Description. Depending on the number of instances you spin up through ElasticBox, each instance is assigned a description that has the format of dasherized-instance-name-datacenter-service_ID-machine-number.


**Deployment**

+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Option                           | Description                                                                                                                |
+==================================+============================================================================================================================+
| Provider                         | Select a CenturyLink Cloud account registered in ElasticBox.                                                               |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+

**Resource**

+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Option                           | Description                                                                                                                |
+==================================+============================================================================================================================+
| Datacenter                       | Select a location to place the instance, for example, UC1.                                                                 |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Group                            | Select placement group for the new instance.                                                                               |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Template                         | Select from a list of CenturyLink Cloud Linux or Windows images. Images are specific to the box service type, that is,     |
|                                  | Linux or Windows.                                                                                                          |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Managed                          | Allow CenturyLink manage this server.                                                                                      |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Instances                        | Specify the number of instances to provision.                                                                              |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+

**Network**

+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Option                           | Description                                                                                                                |
+==================================+============================================================================================================================+
| Network                          | Select a vlan for the new instance.                                                                                        |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Public IP                        |  Check the box to attach a public IP address to the new instance.                                                          |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+

**Compute**

+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Option                           | Description                                                                                                                |
+==================================+============================================================================================================================+
| CPUs                             | Select virtual CPUs for the instance. You can get up to 16 cores.                                                          |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+
| Memory                           | Allocate RAM for the instance. You can get up to 128 GB.                                                                   |
+----------------------------------+----------------------------------------------------------------------------------------------------------------------------+

**Disks**

By default, the machine is provisioned with 17GB local disk space. You can add more disks in RAW format or Partioned, up to 1024 GB.

Auto-Discover Instances
-----------------------------

ElasticBox can auto-discover your existing CenturyLink Cloud VM instances that have been provisioned directly using the provider console outside of ElasticBox. With this capability, even if some of your teams are using CenturyLink Cloud Console to provision instances, you can import them into ElasticBox and manage their lifecycle and also view them as part of the Admin Console Cloud Reports. The discovered instances will exist only as an instance. ElasticBox does not create a corresponding Deployment Policy as part of registration process.

When you add CenturyLink Cloud as a provider for the first time
`````````````````````````````````````````````````````````````````
As soon as you add CenturyLink Cloud as a provider in your workspace, ElasticBox will auto-discover those instances that exist in CenturyLink Cloud and save them in the Unregistered instances tab under the Provider details. You can follow the on-screen instructions to register them in ElasticBox.

If you have existing CenturyLink Cloud provider in ElasticBox
```````````````````````````````````````````````````````````````
The next time you click on sync, ElasticBox will auto-discover those instances that exist in CenturyLink Cloud but have not been provisioned using ElasticBox and save them in the Unregistered instances tab under the Provider details. You can follow the on-screen instructions to register them in ElasticBox.

Auto-discover and register CenturyLink Cloud Server instances in ElasticBox
`````````````````````````````````````````````````````````````````````````````

**Steps**

1. .. raw:: html

	<div class="doc-image padding-1x">
		<img alt="Auto-discover CLC instances" class="img-responsive" src="/../assets/img/docs/providers/auto-discover-clc-instances.png">
	</div>

2. .. raw:: html

	<div class="doc-image padding-1x">
		<img alt="Register CLC instance" class="img-responsive" src="/../assets/img/docs/providers/register-clc-instance.png">
	</div>

3. .. raw:: html

	<div class="doc-image padding-1x">
		<img alt="Registering CLC instance" class="img-responsive" src="/../assets/img/docs/providers/registering-clc-instance.png">
	</div>

4. .. raw:: html

	<div class="doc-image padding-1x">
		<img alt="Registered CLC instance" class="img-responsive" src="/../assets/img/docs/providers/registered-clc-instance.png">
	</div>
