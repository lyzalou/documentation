Configuring the Appliance Network Settings
*************************************************

Once up and running, configure how the appliance connects to the network. By default, it tries to get a dynamic IP address via DHCP. But we recommend that you set a static IP address with these steps so that ElasticBox services are always available at the same address to users.

**Steps**

1. Locate the VM console where you installed the appliance in vCenter or OpenStack.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/appliance/appliance-console-openfromvsphereclient.png" alt="Open the Appliance Console">
		</div>

2. In the Advanced Menu, select **Networking** > **Static IP**.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/appliance/appliance-console-selectstatsicip.png" alt="Select Static IP from the Appliance Console Networking Menu">
		</div>

3. Enter settings based on your network configuration to set up a static IP address for the appliance. When done, press **Apply**.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/appliance/appliance-console-selectstatsicip.png" alt="Select Static IP from the Appliance Console Networking Menu">
		</div>

	* **IP Address**. Unique IPv4 address the appliance needs to connect to the network.
	* **Netmask**. Subnet mask or address of nearest local area network, for example, 255.255.255.0.
	* **Default Gateway**. Enter the subnet router’s IP address. This is similar to the IP address except for the last digit.
	* **Name Server**. Enter the preferred and alternate DNS server addresses in each name server field.

4. Reboot the appliance to apply the configuration changes. In the appliance console, press **Advanced Menu** > **Reboot**.

