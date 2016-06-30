Integrate Network Management Solutions with Webhooks
***********************************************************

ElasticBox integrates with IPAM, DNS, DHCP, and CMDB network management solutions over webhooks. Webhooks are available in the Enterprise Edition.

A webhook requires a custom web service to connect to your network management solution. You can build this custom web service as a box. We show how this works for Infoblox as an example. Say you deploy an instance to vCenter from ElasticBox. ElasticBox calls the custom Infoblox web service over the webhook and provides the instance IP configuration. The web service passes it to Infoblox. Once the web service gets the IP and domain name from Infoblox, it returns the information to ElasticBox, which assigns the IP configuration to the instance.

**In this article:**

* `Integrating with Infoblox`_

Integrating with Infoblox
---------------------------------------------

In this example, Infoblox provides a static IP and domain name for every instance you deploy through ElasticBox in vCenter.

Before You Begin
``````````````````

* Set up a custom specification in vCenter for Linux and Windows if deploying to both platforms.
* Install Infoblox and set it up as follows:

	1. Create a network for every vCenter network to which you deploy from ElasticBox. This has the CIDR range of IP addresses Infoblox can assign.

		Under Data Management > Toolbar, click Add and Add Network.

	2. Add global extensible attributes for the vCenter networks.

		Under Administration > Extensible Attributes, add these four:

		.. raw:: html

			<div class="doc-image padding-1x">
				<div class="browser-feature">
					<div class="indicators">
						<div class="circle magenta"></div>
						<div class="circle orange"></div>
						<div class="circle green"></div>
					</div>
					<div class="browser-window">
						<img class="img-responsive" src="/../assets/img/docs/adminconsole/webhooks-infoblox-add-extensibleattributes-globally.png" alt="Add Extensible Attributes Globally in Infoblox">
					</div>
				</div>
			</div>

		* **Network**. Name of the vCenter network.
		* **Gateway**. Address of the vCenter network gateway node.
		* **Netmask**. Subnet address for the vCenter network.
		* **DNS Suffix**. Domain name suffix that registers and resolves the DNS name.

	3. Add the four global extensible attributes to each network.

		Edit each network. Under Extensible Attributes, add the four attributes and specify values.

		.. raw:: html

			<div class="doc-image padding-1x">
				<div class="browser-feature">
					<div class="indicators">
						<div class="circle magenta"></div>
						<div class="circle orange"></div>
						<div class="circle green"></div>
					</div>
					<div class="browser-window">
						<img class="img-responsive" src="/../assets/img/docs/adminconsole/webhooks-infoblox-add-extensibleattributes-to-network.png" alt="Add Extensible Attributes to the Network in Infoblox">
					</div>
				</div>
			</div>

		When you deploy, ElasticBox maps the network you select in the deployment policy to these Infoblox network attributes to derive the right IP configuration.

Steps
`````````

1. Create a custom Infoblox web service.

	We define the web service in a box. `Contact us`_ to share the box with you.

	.. _Contact us: support@elasticbox.com

	.. raw:: html

			<div class="doc-image padding-1x">
				<div class="browser-feature">
					<div class="indicators">
						<div class="circle magenta"></div>
						<div class="circle orange"></div>
						<div class="circle green"></div>
					</div>
					<div class="browser-window">
						<img class="img-responsive" src="/../assets/img/docs/adminconsole/webhooks-infoblox-customwebservice-box.png" alt="Infoblox Custom Web Service Box">
					</div>
				</div>
			</div>

	**Install script**

	This box installs Python, Python dependencies, and the Apache web server.

	.. raw:: html

			<pre>
			#!/bin/bash

			yum -y install httpd mod_wsgi python-virtualenv
			curl -k https://bootstrap.pypa.io/get-pip.py | python

			pip install --upgrade bottle
			pip install --upgrade requests
			</pre>

	**Configure script**

	The box configures Apache using the virtual.conf file variable. To create the web service endpoint, it runs the webhook.py script from a file variable.

	.. raw:: html

			<pre>
			#!/bin/bash

			mkdir -p /var/www/webhook

			curl -k {{ VIRTUAL_CONF }} -o /etc/httpd/conf.d/default.conf
			curl -k {{ WEBHOOK_PY }} | elasticbox config -o /var/www/webhook/webhook.py
			</pre>

	Here's the Python script to create the web service endpoint:

	.. raw:: html

			<pre>
			#!/usr/bin/python
			# -*- coding: utf-8 -*-

			'''
			ElasticBox Confidential
			Copyright (c) 2013 All Rights Reserved, ElasticBox Inc.
			    
			NOTICE:  All information contained herein is, and remains the property
			of ElasticBox. The intellectual and technical concepts contained herein are
			proprietary and may be covered by U.S. and Foreign Patents, patents in process,
			and are protected by trade secret or copyright law. Dissemination of this
			information or reproduction of this material is strictly forbidden unless prior
			written permission is obtained from ElasticBox
			'''

			import json
			import bottle
			import requests

			from bottle import route, run, request


			def allocate_address_with_infoblox(network, name):
			    net_url = \
			        'https:///wapi/v1.4/network?_return_fields=extattrs&*Network=%s'
			    ip_url = \
			        'https:///wapi/v1.4/%s?_function=next_available_ip&num=1'
			    a_record_url = 'https:///wapi/v1.4/record:a'
			    ptr_record_url = \
			        'https:///wapi/v1.4/record:ptr'

			    auth = ('', '')

			    networks = requests.get(net_url % network, auth=auth,
			                            verify=False).json()

			    if len(networks) > 0:
			        address = requests.post(ip_url % networks[0]['_ref'],
			                                auth=auth, verify=False).json()['ips'
			                ][0]

			        dns_suffix = networks[0]['extattrs']['Suffix']['value']
			        name = '{0}.{1}'.format(dns_suffix, name)

			        a_record = {'ipv4addr': address, 'name': name,
			                    'view': 'default'}
			        ptr_record = {'ipv4addr': address, 'ptrdname': name,
			                      'view': 'default'}

			        a_response = requests.post(a_record_url, auth=auth,
			                                   data=json.dumps(a_record),
			                                   verify=False)
			        ptr_response = requests.post(ptr_record_url, auth=auth,
			                data=json.dumps(ptr_record), verify=False)

			        return {
			            'ipv4_address': address,
			            'subnet_mask': networks[0]['extattrs']['Netmask']['value'],
			            'default_gateway': networks[0]['extattrs']['Gateway'
			                    ]['value'],
			            'preferred_nameserver': '8.8.8.8',
			            }


			def search(items, name):
			    for item in items:
			        if item['name'] == name:
			            return item['value']

			    return ''


			@route('/requestIP', method='POST')
			def requestIP():
			    body = json.load(request.body)
			    machine = body['machine']
			    service = body['service']
			    instance = body['instance']

			    variables = []

			    if 'vsphere' in machine['schema']:
			        if instance['operation'] == 'terminate':
			            print 'Deallocate IP address'
			            return {}

			        is_infoblox = True
			        variables = instance['variables']
			        for variable in instance['variables']:
			            if variable['name'] == 'IPV4_ADDRESS' and variable['value'] \
			                != '':
			                is_infoblox = False

			        if is_infoblox:
			            network = allocate_address_with_infoblox(service['profile'
			                    ]['network'], machine['name'])
			            if network:
			                machine['customization'] = \
			                    {'instance_networks': [network]}
			        else:
			            nameservers = find(variables, 'NAME_SERVERS')
			            preferred_nameserver = nameservers.split(',')[0]
			            alternate_nameserver = nameservers.split(',')[1]

			            machine['customization'] = {
			                'ipv4_address': find(variables, 'IPV4_ADDRESS'),
			                'subnet_mask': find(variables, 'SUBNET_MASK'),
			                'default_gateway': find(variables, 'DEFAULT_GATEWAY'),
			                'preferred_nameserver': preferred_nameserver,
			                'alternate_nameserver': alternate_nameserver,
			                'dns_suffixes': find(variables, 'DNS_SUFFIXES'
			                        ).split(','),
			                }

			    return machine


			@route('/test/<network>/<name>', method='GET')
			def testIP(network, name):
			    return allocate_address_with_infoblox(network, name)

			application = bottle.default_app()
			</pre>

2. Deploy the custom Infoblox web service box.

	We provide values for the address, password, and user. These are admin credentials to access Infoblox.

	.. raw:: html

			<div class="doc-image padding-1x">
      			<img class="img-responsive" src="/../assets/img/docs/adminconsole/webhooks-infoblox-webservicebox-deploy.png" alt="Deploy the Infoblox Web Service">
			</div>

3. Add the custom Infoblox web service endpoint as a webhook.

	Under `Admin Console </../documentation/managing-your-organization/admin-overview/>`_ > Webhooks, enter the endpoint of the deployed webservice as a webhook like this: **http://endpoint_of_webservice_instance/requestIP**

	.. raw:: html

			<div class="doc-image padding-1x">
      			<img class="img-responsive" src="/../assets/img/docs/adminconsole/webhooks-adminconsole-add.png" alt="Add Webhook to the Infoblox Web Service">
			</div>

4. To see the Infoblox integration in action, we deploy an instance from ElasticBox to vCenter.

	In the deployment policy, we select a custom specification. This acts as a holder for the network information ElasticBox gets from InfoBlox. vCenter overrides the values of the custom specification with the values from Infoblox.