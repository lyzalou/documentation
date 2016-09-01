Organizations API
*****************

Manage organizations.

**Manage an Organization**

+-------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Resource                                                    | Description                                                                                                                                                                                                                            |
+=============================================================+========================================================================================================================================================================================================================================+
| `GET /services/organizations/{organization_name}`_          | Gets the schema of the given organization.                                                                                                                                                                                             |
+-------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| `PUT /services/organizations/{organization_name}`_          | Updates an existing organization.                                                                                                                                                                                                      |
+-------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| `PUT /organizations/{organization_name}/sync_groups`_       | Queues a request to sync LDAP groups.                                                                                                                                                                                                  |
+-------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+


GET /services/organizations/{organization_name}
-----------------------------------------------

Gets the schema of a given organization.

**Normal Response Codes**

* 200

**Common Error Response Codes**

* User doesn't belong to the organization (403)
* Not Found (404)

**Request Headers**

.. raw:: html

	<pre>
	Content-Type: application/json
	Elasticbox-Token: your_authentication_token
	ElasticBox-Release: 4.0
	</pre>

**Response Parameters**

+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter                  | Type                 | Description                                                                                                                                                                             |
+============================+======================+=========================================================================================================================================================================================+
| schema                     | string               | Organization schema URI:                                                                                                                                                                |
|                            |                      | **http://elasticbox.net/schemas/organization**                                                                                                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| name                       | string               | Organization name.                                                                                                                                                                      |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| icon                       | string               | Organization icon URI.                                                                                                                                                                  |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| updated                    | string               | Date of the last update.                                                                                                                                                                |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| created                    | string               | Creation date.                                                                                                                                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| setup                      | boolean              | This is read-only. It indicates that the ElasticBox appliance is set up and ready for use.                                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| administrators             | array                | List of users who can administer the organization.                                                                                                                                      |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| domains                    | string               | Domains that are a part of the organization.                                                                                                                                            |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| authentication             | object               | List of the authentication methods to allow single sign-on in the organization. Contains the following properties:                                                                      |
|                            |                      |  * github: Boolean. If enabled, it is true, else false.                                                                                                                                 |
|                            |                      |  * google: Boolean. If enabled, it is true, else false.                                                                                                                                 |
|                            |                      |  * password: Boolean. If enabled, it is true, else false.                                                                                                                               |
|                            |                      |  * ldap: Boolean. If enabled, it is true, else false.                                                                                                                                   |
|                            |                      |  * ldap_config: Object that contains the LDAP service settings:                                                                                                                         |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * ldap_group_sync: Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                         |
|                            |                      |    * sources: Array of `LDAP sources </../documentation/managing-your-organization/user-authentication/#userauth-ldap-setup>`_. Each source has the following properties:               |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * host: Required. String identifies the hostname or IP address of the LDAP service.                                                                                              |
|                            |                      |        * groups_dn: String specifies a fully qualified group name.                                                                                                                      |
|                            |                      |        * group_dn_filter: String defines an entity on the LDAP server. All groups are synchronised as children of this entity.                                                          |
|                            |                      |        * email_field: String specifies the email field name by which to look up users. Typically, this field is called email.                                                           |
|                            |                      |        * ldap_search_password: String specifies the password for the LDAP service account to look up users who try to log in.                                                           |
|                            |                      |        * ldap_search_user: String specifies the username of the LDAP service account to look up users who try to log in.                                                                |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ldap_last_sync_completed   | string               | Timestamp of the last successful LDAP group sync, for example, 2015-04-06 14:28:12.874910. Value is null if ldap_group_sync is set to false.                                            |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ldap_groups                | array                | List of objects, each of which is an LDAP group. Each group has two properties:                                                                                                         |
|                            |                      |  * dn: String identifier for the group.                                                                                                                                                 |
|                            |                      |  * name: String name shown in the workspace web interface.                                                                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| providers                  | array                | List of cloud providers the organization can enable to register and deploy. Each provider type has the following properties:                                                            |
|                            |                      |  * enabled: Boolean value of true if enabled, else false.                                                                                                                               |
|                            |                      |  * type: String values of the supported cloud providers: Amazon Web Services, Openstack, VMWare vSphere, Google Compute, Microsoft Azure, Cloudstack, SoftLayer, VMware vCloud Director,|
|                            |                      |    Amazon Web Services GovCloud, Rackspace.                                                                                                                                             |
|                            |                      |  * description: String that briefly enumerates the services from the cloud provider.                                                                                                    |
|                            |                      |  * pricing: Array of pricing information for Linux and Windows compute instance types. Only available for Amazon Web Services.                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| tags                       | array                | List of `tags </../documentation/managing-your-organization/resource-tags/#preset>`_ applied on instances deployed to cloud providers from the organization. Each tag has three         |
|                            |                      | properties:                                                                                                                                                                             |
|                            |                      |  * name: String you apply as a tag.                                                                                                                                                     |
|                            |                      |  * type: String identifies the type of tag whether an ElasticBox object or a custom one. Allowed values are Box, Workspace, Provider, Environment, Email, User ID, Service Instance ID, |
|                            |                      |    Service ID, Workspace ID, Instance ID, Custom.                                                                                                                                       |
|                            |                      |  * value: String value of null for ElasticBox objects. For custom tags, set its value using this property.                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| webhooks                   | array                | List of webhooks that integrate with the organization.                                                                                                                                  |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| cost_centers               | array                | List of cost centers. Each cost center contains the following properties:                                                                                                               |
|                            |                      |  * enforce: Boolean. If true, an instance cannot be deployed if it is over the quota.                                                                                                   |
|                            |                      |  * name: String. Name of the cost center                                                                                                                                                |
|                            |                      |  * workspaces: Array. List of the names that belongs to the cost center.                                                                                                                |
|                            |                      |  * quotas: List of quotas. Each quota contains an object with the following properties:                                                                                                 |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * cost: Required. Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                          |
|                            |                      |    * provider: Required. Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                      |
|                            |                      |    * allocated: Array. List of instances which are contributing to the current quota. Each allocated instance has these properties:                                                     |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * instance_id: Required. String. Id of the instance.                                                                                                                             |
|                            |                      |        * instances: Required. Int. Number of instances.                                                                                                                                 |
|                            |                      |        * started: Required. String. Date when this instance was deployed.                                                                                                               |
|                            |                      |        * flavor: Required. String. Type of instance.                                                                                                                                    |
|                            |                      |        * region: Required. String. Region where it was deployed.                                                                                                                        |
|                            |                      |        * service_type: Required. String. Type of the service.                                                                                                                           |
|                            |                      |        * terminated: String specifies the username of the LDAP service account to look up users who try to log in.                                                                      |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * resources: Object. Resources of the quota.                                                                                                                                         |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * cpu: Required. Int. Number of cpu units.                                                                                                                                       |
|                            |                      |        * disk: Required. Object. A disk with these properties:                                                                                                                          |
|                            |                      |           * quantity: Required. String. Amount of storage.                                                                                                                              |
|                            |                      |           * unit: Required. String. Mb, Gb or Tb.                                                                                                                                       |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * ram: Required. String. Ram of the quota.                                                                                                                                       |
|                            |                      |           * quantity: Required. String. Amount of storage.                                                                                                                              |
|                            |                      |           * unit: Required. String. Mb or Gb.                                                                                                                                           |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Response Body**

.. raw:: html

	<pre>
	{
	   {
	   "schema":"http://elasticbox.net/schemas/organization",
	   "name":"elasticbox",
	   "icon":"/services/blobs/download/5452705c3bbd224ef9541c41/elasticbox.png",
	   "theme":null,
	   "updated":"2015-04-06 14:28:12.874910",
	   "created":"2014-02-14 15:12:21.526672",
	   "setup":true,
	   "administrators":[
	      "x",
	      "a",
	      "bc",
	      "ca",
	      "ce",
	      "di",
	      "el",
	      "ig",
	      "la",
	      "ma",
	      "mas",
	      "mr",
	      "os",
	      "ra",
	      "ri",
	      "ri",
	      "ys",
	      "lu"
	   ],
	   "domains":[
	      "elasticbox.com"
	   ],
	   "authentication":{
	      "github":false,
	      "google":true,
	      "ldap":true,
	      "password":false,
	      "ldap_config":{
	         "sources":[
	            {
	               "host":"ldap://ldap.elasticbox.com",
	               "email_field":"mail"
	            }
	         ]
	      }
	   },
	   "features":{
	      "admin_boxes":true,
	      "cost_center":true,
	      "custom_pricing":false,
	      "onboard_checklist":false,
	      "provider_sharing":true,
	      "reporting":true
	   },
	   "providers":[
	      {
	         "enabled":true,
	         "type":"Amazon Web Services",
	         "description":"Manage EC2, S3, Dynamo DB, and RDS instances",
	         "pricing":[
	            {
	               "platform":"Linux Compute",
	               "price":7000,
	               "region":"ap-southeast-2",
	               "flavor":"i2.8xlarge",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            },
	            {
	               "platform":"Linux Compute",
	               "price":5,
	               "region":"us-east-1",
	               "flavor":"t2.micro",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            },
	            {
	               "platform":"Windows Compute",
	               "price":10,
	               "region":"us-east-1",
	               "flavor":"t2.micro",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            }
	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Openstack",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"VMware vSphere",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Google Compute",
	         "description":"Manage cloud hosting and Linux machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Microsoft Azure",
	         "description":"Manage compute services for Windows and Linux machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Cloudstack",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"SoftLayer",
	         "description":"Manage compute services for Windows and Linux machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"VMware vCloud Director",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Amazon Web Services GovCloud",
	         "description":"Manage compute services in an isolated ITAR compliant AWS region.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Rackspace",
	         "description":"Manage cloud hosting and Linux machines.",
	         "pricing":[

	         ]
	      }
	   ],
	   "tags":[
	      {
	         "name":"workspace",
	         "type":"Workspace",
	         "value":null
	      },
	      {
	         "name":"box",
	         "type":"Box",
	         "value":null
	      },
	      {
	         "name":"environment",
	         "type":"Environment",
	         "value":null
	      },
	      {
	         "name":"email",
	         "type":"Email",
	         "value":null
	      },
	      {
	         "name":"user",
	         "type":"User ID",
	         "value":null
	      },
	      {
	         "name":"Name",
	         "type":"Service Instance ID",
	         "value":null
	      }
	   ],
	   "cost_centers":[
	      {
	         "name":"test",
	         "enforce":false,
	         "quotas":[
	            {
	               "allocated":[

	               ],
	               "cost":0,
	               "provider":"2bf1bd2c-b03d-460f-80da-647d26bdbcfe"
	            },
	            {
	               "cost":3000,
	               "provider":"5908ee9b-0c0a-4af6-8eef-2dc9f95d033a"
	            }
	         ],
	         "workspaces":[
	            "operations"
	         ]
	      }
	   ],
	   "webhooks":[

	   ]
	}
	</pre>

PUT /services/organizations/{organization_name}
-----------------------------------------------

Updates an existing organization given its name. Only the organization administrator can update.

**Normal Response Codes**

* 200

**Common Error Response Codes**

* User doesn't belong to the organization (403)
* Not Found (404)

**Request Headers**

.. raw:: html

	<pre>
	Content-Type: application/json
	Elasticbox-Token: your_authentication_token
	ElasticBox-Release: 4.0
	</pre>

**Request Body**

.. raw:: html

	<pre>
	{
	   "schema":"http://elasticbox.net/schemas/organization",
	   "name":"elasticbox",
	   "icon":"/services/blobs/download/5452705c3bbd224ef9541c41/elasticbox.png",
	   "theme":null,
	   "updated":"2015-04-06 14:28:12.874910",
	   "created":"2014-02-14 15:12:21.526672",
	   "setup":true,
	   "administrators":[
	      "ad",
	      "al",
	      "ar",
	      "ca",
	      "ce",
	      "di",
	      "el",
	      "ig",
	      "la",
	      "ma",
	      "mas",
	      "mr",
	      "os",
	      "ra",
	      "ri",
	      "ric",
	      "ys",
	      "lu"
	   ],
	   "domains":[
	      "elasticbox.com"
	   ],
	   "authentication":{
	      "github":false,
	      "google":true,
	      "ldap":true,
	      "password":false,
	      "username":null,
	      "ldap_config":{
	         "sources":[
	            {
	               "host":"ldap://ldap.elasticbox.com",
	               "email_field":"mail"
	            }
	         ]
	      }
	   },
	   "features":{
	      "admin_boxes":true,
	      "cost_center":true,
	      "custom_pricing":false,
	      "onboard_checklist":false,
	      "provider_sharing":true,
	      "reporting":true
	   },
	   "providers":[
	      {
	         "enabled":true,
	         "type":"Amazon Web Services",
	         "description":"Manage EC2, S3, Dynamo DB, and RDS instances",
	         "pricing":[
	            {
	               "platform":"Linux Compute",
	               "price":7000,
	               "region":"ap-southeast-2",
	               "flavor":"i2.8xlarge",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            },
	            {
	               "platform":"Linux Compute",
	               "price":5,
	               "region":"us-east-1",
	               "flavor":"t2.micro",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            },
	            {
	               "platform":"Windows Compute",
	               "price":10,
	               "region":"us-east-1",
	               "flavor":"t2.micro",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            }
	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Openstack",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"VMware vSphere",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Google Compute",
	         "description":"Manage cloud hosting and Linux machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Microsoft Azure",
	         "description":"Manage compute services for Windows and Linux machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Cloudstack",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"SoftLayer",
	         "description":"Manage compute services for Windows and Linux machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"VMware vCloud Director",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Amazon Web Services GovCloud",
	         "description":"Manage compute services in an isolated ITAR compliant AWS region.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Rackspace",
	         "description":"Manage cloud hosting and Linux machines.",
	         "pricing":[

	         ]
	      }
	   ],
	   "tags":[
	      {
	         "name":"workspace",
	         "type":"Workspace",
	         "value":null
	      },
	      {
	         "name":"box",
	         "type":"Box",
	         "value":null
	      },
	      {
	         "name":"environment",
	         "type":"Environment",
	         "value":null
	      },
	      {
	         "name":"email",
	         "type":"Email",
	         "value":null
	      },
	      {
	         "name":"user",
	         "type":"User ID",
	         "value":null
	      },
	      {
	         "name":"Name",
	         "type":"Service Instance ID",
	         "value":null
	      },
	      {
	         "name":"Testing",
	         "type":"Custom",
	         "value":"test"
	      }
	   ],
	   "cost_centers":[
	      {
	         "name":"test",
	         "enforce":false,
	         "quotas":[
	            {
	               "allocated":[

	               ],
	               "cost":0,
	               "provider":"2bf1bd2c-b03d-460f-80da-647d26bdbcfe"
	            },
	            {
	               "cost":3000,
	               "provider":"5908ee9b-0c0a-4af6-8eef-2dc9f95d033a"
	            }
	         ],
	         "workspaces":[
	            "operations"
	         ]
	      }
	   ],
	   "webhooks":[

	   ]
	}
	</pre>

**Request parameters**

+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter                  | Type                 | Description                                                                                                                                                                             |
+============================+======================+=========================================================================================================================================================================================+
| schema                     | string               | Organization schema URI:                                                                                                                                                                |
|                            |                      | **http://elasticbox.net/schemas/organization**                                                                                                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| name                       | string               | Organization name.                                                                                                                                                                      |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| icon                       | string               | Organization icon URI.                                                                                                                                                                  |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| updated                    | string               | Date of the last update.                                                                                                                                                                |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| created                    | string               | Creation date.                                                                                                                                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| setup                      | boolean              | This is read-only. It indicates that the ElasticBox appliance is set up and ready for use.                                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| administrators             | array                | List of users who can administer the organization.                                                                                                                                      |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| domains                    | string               | Domains that are a part of the organization.                                                                                                                                            |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| authentication             | object               | List of the authentication methods to allow single sign-on in the organization. Contains the following properties:                                                                      |
|                            |                      |  * github: Boolean. If enabled, it is true, else false.                                                                                                                                 |
|                            |                      |  * google: Boolean. If enabled, it is true, else false.                                                                                                                                 |
|                            |                      |  * password: Boolean. If enabled, it is true, else false.                                                                                                                               |
|                            |                      |  * ldap: Boolean. If enabled, it is true, else false.                                                                                                                                   |
|                            |                      |  * ldap_config: Object that contains the LDAP service settings:                                                                                                                         |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * ldap_group_sync: Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                         |
|                            |                      |    * sources: Array of `LDAP sources </../documentation/managing-your-organization/user-authentication/#userauth-ldap-setup>`_. Each source has the following properties:               |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * host: Required. String identifies the hostname or IP address of the LDAP service.                                                                                              |
|                            |                      |        * groups_dn: String specifies a fully qualified group name.                                                                                                                      |
|                            |                      |        * group_dn_filter: String defines an entity on the LDAP server. All groups are synchronised as children of this entity.                                                          |
|                            |                      |        * email_field: String specifies the email field name by which to look up users. Typically, this field is called email.                                                           |
|                            |                      |        * ldap_search_password: String specifies the password for the LDAP service account to look up users who try to log in.                                                           |
|                            |                      |        * ldap_search_user: String specifies the username of the LDAP service account to look up users who try to log in.                                                                |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ldap_last_sync_completed   | string               | Timestamp of the last successful LDAP group sync, for example, 2015-04-06 14:28:12.874910. Value is null if ldap_group_sync is set to false.                                            |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ldap_groups                | array                | List of objects, each of which is an LDAP group. Each group has two properties:                                                                                                         |
|                            |                      |  * dn: String identifier for the group.                                                                                                                                                 |
|                            |                      |  * name: String name shown in the workspace web interface.                                                                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| providers                  | array                | List of cloud providers the organization can enable to register and deploy. Each provider type has the following properties:                                                            |
|                            |                      |  * enabled: Boolean value of true if enabled, else false.                                                                                                                               |
|                            |                      |  * type: String values of the supported cloud providers: Amazon Web Services, Openstack, VMWare vSphere, Google Compute, Microsoft Azure, Cloudstack, SoftLayer, VMware vCloud Director,|
|                            |                      |    Amazon Web Services GovCloud, Rackspace.                                                                                                                                             |
|                            |                      |  * description: String that briefly enumerates the services from the cloud provider.                                                                                                    |
|                            |                      |  * pricing: Array of pricing information for Linux and Windows compute instance types. Only available for Amazon Web Services.                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| tags                       | array                | List of `tags </../documentation/managing-your-organization/resource-tags/#preset>`_ applied on instances deployed to cloud providers from the organization. Each tag has three         |
|                            |                      | properties:                                                                                                                                                                             |
|                            |                      |  * name: String you apply as a tag.                                                                                                                                                     |
|                            |                      |  * type: String identifies the type of tag whether an ElasticBox object or a custom one. Allowed values are Box, Workspace, Provider, Environment, Email, User ID, Service Instance ID, |
|                            |                      |    Service ID, Workspace ID, Instance ID, Custom.                                                                                                                                       |
|                            |                      |  * value: String value of null for ElasticBox objects. For custom tags, set its value using this property.                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| webhooks                   | array                | List of webhooks that integrate with the organization.                                                                                                                                  |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| cost_centers               | array                | List of cost centers. Each cost center contains the following properties:                                                                                                               |
|                            |                      |  * enforce: Boolean. If true, an instance cannot be deployed if it is over the quota.                                                                                                   |
|                            |                      |  * name: String. Name of the cost center                                                                                                                                                |
|                            |                      |  * workspaces: Array. List of the names that belongs to the cost center.                                                                                                                |
|                            |                      |  * quotas: List of quotas. Each quota contains an object with the following properties:                                                                                                 |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * cost: Required. Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                          |
|                            |                      |    * provider: Required. Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                      |
|                            |                      |    * allocated: Array. List of instances which are contributing to the current quota. Each allocated instance has these properties:                                                     |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * instance_id: Required. String. Id of the instance.                                                                                                                             |
|                            |                      |        * instances: Required. Int. Number of instances.                                                                                                                                 |
|                            |                      |        * started: Required. String. Date when this instance was deployed.                                                                                                               |
|                            |                      |        * flavor: Required. String. Type of instance.                                                                                                                                    |
|                            |                      |        * region: Required. String. Region where it was deployed.                                                                                                                        |
|                            |                      |        * service_type: Required. String. Type of the service.                                                                                                                           |
|                            |                      |        * terminated: String specifies the username of the LDAP service account to look up users who try to log in.                                                                      |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * resources: Object. Resources of the quota.                                                                                                                                         |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * cpu: Required. Int. Number of cpu units.                                                                                                                                       |
|                            |                      |        * disk: Required. Object. A disk with these properties:                                                                                                                          |
|                            |                      |           * quantity: Required. String. Amount of storage.                                                                                                                              |
|                            |                      |           * unit: Required. String. Mb, Gb or Tb.                                                                                                                                       |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * ram: Required. String. Ram of the quota.                                                                                                                                       |
|                            |                      |           * quantity: Required. String. Amount of storage.                                                                                                                              |
|                            |                      |           * unit: Required. String. Mb or Gb.                                                                                                                                           |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Response parameters**


+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter                  | Type                 | Description                                                                                                                                                                             |
+============================+======================+=========================================================================================================================================================================================+
| schema                     | string               | Organization schema URI:                                                                                                                                                                |
|                            |                      | **http://elasticbox.net/schemas/organization**                                                                                                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| name                       | string               | Organization name.                                                                                                                                                                      |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| icon                       | string               | Organization icon URI.                                                                                                                                                                  |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| updated                    | string               | Date of the last update.                                                                                                                                                                |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| created                    | string               | Creation date.                                                                                                                                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| setup                      | boolean              | This is read-only. It indicates that the ElasticBox appliance is set up and ready for use.                                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| administrators             | array                | List of users who can administer the organization.                                                                                                                                      |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| domains                    | string               | Domains that are a part of the organization.                                                                                                                                            |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| authentication             | object               | List of the authentication methods to allow single sign-on in the organization. Contains the following properties:                                                                      |
|                            |                      |  * github: Boolean. If enabled, it is true, else false.                                                                                                                                 |
|                            |                      |  * google: Boolean. If enabled, it is true, else false.                                                                                                                                 |
|                            |                      |  * password: Boolean. If enabled, it is true, else false.                                                                                                                               |
|                            |                      |  * ldap: Boolean. If enabled, it is true, else false.                                                                                                                                   |
|                            |                      |  * ldap_config: Object that contains the LDAP service settings:                                                                                                                         |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * ldap_group_sync: Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                         |
|                            |                      |    * sources: Array of `LDAP sources </../documentation/managing-your-organization/user-authentication/#userauth-ldap-setup>`_. Each source has the following properties:               |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * host: Required. String identifies the hostname or IP address of the LDAP service.                                                                                              |
|                            |                      |        * groups_dn: String specifies a fully qualified group name.                                                                                                                      |
|                            |                      |        * group_dn_filter: String defines an entity on the LDAP server. All groups are synchronised as children of this entity.                                                          |
|                            |                      |        * email_field: String specifies the email field name by which to look up users. Typically, this field is called email.                                                           |
|                            |                      |        * ldap_search_password: String specifies the password for the LDAP service account to look up users who try to log in.                                                           |
|                            |                      |        * ldap_search_user: String specifies the username of the LDAP service account to look up users who try to log in.                                                                |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ldap_last_sync_completed   | string               | Timestamp of the last successful LDAP group sync, for example, 2015-04-06 14:28:12.874910. Value is null if ldap_group_sync is set to false.                                            |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ldap_groups                | array                | List of objects, each of which is an LDAP group. Each group has two properties:                                                                                                         |
|                            |                      |  * dn: String identifier for the group.                                                                                                                                                 |
|                            |                      |  * name: String name shown in the workspace web interface.                                                                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| providers                  | array                | List of cloud providers the organization can enable to register and deploy. Each provider type has the following properties:                                                            |
|                            |                      |  * enabled: Boolean value of true if enabled, else false.                                                                                                                               |
|                            |                      |  * type: String values of the supported cloud providers: Amazon Web Services, Openstack, VMWare vSphere, Google Compute, Microsoft Azure, Cloudstack, SoftLayer, VMware vCloud Director,|
|                            |                      |    Amazon Web Services GovCloud, Rackspace.                                                                                                                                             |
|                            |                      |  * description: String that briefly enumerates the services from the cloud provider.                                                                                                    |
|                            |                      |  * pricing: Array of pricing information for Linux and Windows compute instance types. Only available for Amazon Web Services.                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| tags                       | array                | List of `tags </../documentation/managing-your-organization/resource-tags/#preset>`_ applied on instances deployed to cloud providers from the organization. Each tag has three         |
|                            |                      | properties:                                                                                                                                                                             |
|                            |                      |  * name: String you apply as a tag.                                                                                                                                                     |
|                            |                      |  * type: String identifies the type of tag whether an ElasticBox object or a custom one. Allowed values are Box, Workspace, Provider, Environment, Email, User ID, Service Instance ID, |
|                            |                      |    Service ID, Workspace ID, Instance ID, Custom.                                                                                                                                       |
|                            |                      |  * value: String value of null for ElasticBox objects. For custom tags, set its value using this property.                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| webhooks                   | array                | List of webhooks that integrate with the organization.                                                                                                                                  |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| cost_centers               | array                | List of cost centers. Each cost center contains the following properties:                                                                                                               |
|                            |                      |  * enforce: Boolean. If true, an instance cannot be deployed if it is over the quota.                                                                                                   |
|                            |                      |  * name: String. Name of the cost center                                                                                                                                                |
|                            |                      |  * workspaces: Array. List of the names that belongs to the cost center.                                                                                                                |
|                            |                      |  * quotas: List of quotas. Each quota contains an object with the following properties:                                                                                                 |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * cost: Required. Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                          |
|                            |                      |    * provider: Required. Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                      |
|                            |                      |    * allocated: Array. List of instances which are contributing to the current quota. Each allocated instance has these properties:                                                     |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * instance_id: Required. String. Id of the instance.                                                                                                                             |
|                            |                      |        * instances: Required. Int. Number of instances.                                                                                                                                 |
|                            |                      |        * started: Required. String. Date when this instance was deployed.                                                                                                               |
|                            |                      |        * flavor: Required. String. Type of instance.                                                                                                                                    |
|                            |                      |        * region: Required. String. Region where it was deployed.                                                                                                                        |
|                            |                      |        * service_type: Required. String. Type of the service.                                                                                                                           |
|                            |                      |        * terminated: String specifies the username of the LDAP service account to look up users who try to log in.                                                                      |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * resources: Object. Resources of the quota.                                                                                                                                         |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * cpu: Required. Int. Number of cpu units.                                                                                                                                       |
|                            |                      |        * disk: Required. Object. A disk with these properties:                                                                                                                          |
|                            |                      |           * quantity: Required. String. Amount of storage.                                                                                                                              |
|                            |                      |           * unit: Required. String. Mb, Gb or Tb.                                                                                                                                       |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * ram: Required. String. Ram of the quota.                                                                                                                                       |
|                            |                      |           * quantity: Required. String. Amount of storage.                                                                                                                              |
|                            |                      |           * unit: Required. String. Mb or Gb.                                                                                                                                           |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Response Body**

.. raw:: html

	<pre>
	{
	   "schema":"http://elasticbox.net/schemas/organization",
	   "name":"elasticbox",
	   "icon":"/services/blobs/download/5452705c3bbd224ef9541c41/elasticbox.png",
	   "theme":null,
	   "updated":"2015-04-06 20:38:46.060399",
	   "created":"2014-02-14 15:12:21.526672",
	   "setup":true,
	   "administrators":[
	      "ad",
	      "al",
	      "ar",
	      "ca",
	      "ce",
	      "di",
	      "el",
	      "ig",
	      "la",
	      "ma",
	      "mas",
	      "mr",
	      "os",
	      "ra",
	      "ri",
	      "ric",
	      "ys",
	      "lu"
	   ],
	   "domains":[
	      "elasticbox.com"
	   ],
	   "authentication":{
	      "github":false,
	      "google":true,
	      "ldap":true,
	      "password":false,
	      "username":null,
	      "ldap_config":{
	         "sources":[
	            {
	               "host":"ldap://ldap.elasticbox.com",
	               "email_field":"mail"
	            }
	         ]
	      }
	   },
	   "features":{
	      "admin_boxes":true,
	      "cost_center":true,
	      "custom_pricing":false,
	      "onboard_checklist":false,
	      "provider_sharing":true,
	      "reporting":true
	   },
	   "providers":[
	      {
	         "enabled":true,
	         "type":"Amazon Web Services",
	         "description":"Manage EC2, S3, Dynamo DB, and RDS instances",
	         "pricing":[
	            {
	               "platform":"Linux Compute",
	               "price":7000,
	               "region":"ap-southeast-2",
	               "flavor":"i2.8xlarge",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            },
	            {
	               "platform":"Linux Compute",
	               "price":5,
	               "region":"us-east-1",
	               "flavor":"t2.micro",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            },
	            {
	               "platform":"Windows Compute",
	               "price":10,
	               "region":"us-east-1",
	               "flavor":"t2.micro",
	               "schema":"http://elasticbox.net/schemas/aws/compute/pricing"
	            }
	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Openstack",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"VMware vSphere",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Google Compute",
	         "description":"Manage cloud hosting and Linux machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Microsoft Azure",
	         "description":"Manage compute services for Windows and Linux machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Cloudstack",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"SoftLayer",
	         "description":"Manage compute services for Windows and Linux machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"VMware vCloud Director",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Amazon Web Services GovCloud",
	         "description":"Manage compute services in an isolated ITAR compliant AWS region.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Rackspace",
	         "description":"Manage cloud hosting and Linux machines.",
	         "pricing":[

	         ]
	      }
	   ],
	   "tags":[
	      {
	         "name":"workspace",
	         "type":"Workspace",
	         "value":null
	      },
	      {
	         "name":"box",
	         "type":"Box",
	         "value":null
	      },
	      {
	         "name":"environment",
	         "type":"Environment",
	         "value":null
	      },
	      {
	         "name":"email",
	         "type":"Email",
	         "value":null
	      },
	      {
	         "name":"user",
	         "type":"User ID",
	         "value":null
	      },
	      {
	         "name":"Name",
	         "type":"Service Instance ID",
	         "value":null
	      },
	      {
	         "name":"Testing",
	         "type":"Custom",
	         "value":"test"
	      }
	   ],
	   "cost_centers":[
	      {
	         "name":"test",
	         "enforce":false,
	         "quotas":[
	            {
	               "allocated":[

	               ],
	               "cost":0,
	               "provider":"2bf1bd2c-b03d-460f-80da-647d26bdbcfe"
	            },
	            {
	               "cost":3000,
	               "provider":"5908ee9b-0c0a-4af6-8eef-2dc9f95d033a"
	            }
	         ],
	         "workspaces":[
	            "operations"
	         ]
	      }
	   ],
	   "webhooks":[

	   ]
	}
	</pre>

PUT /organizations/{organization_name}/sync_groups
--------------------------------------------------

Queues a request to sync LDAP groups. The sync request, depending on the amount of data from the LDAP service, can take a few minutes. The ldap_last_sync_completed property updates when the request finishes successfully.

**Normal Response Codes**

* 200

**Error Response Codes**

* Not Found (404)

**Request Headers**

.. raw:: html

	<pre>
	Content-Type: application/json
	Elasticbox-Token: your_authentication_token
	ElasticBox-Release: 4.0
	</pre>

**Response Parameters**

+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter                  | Type                 | Description                                                                                                                                                                             |
+============================+======================+=========================================================================================================================================================================================+
| schema                     | string               | Organization schema URI:                                                                                                                                                                |
|                            |                      | **http://elasticbox.net/schemas/organization**                                                                                                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| name                       | string               | Organization name.                                                                                                                                                                      |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| icon                       | string               | Organization icon URI.                                                                                                                                                                  |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| updated                    | string               | Date of the last update.                                                                                                                                                                |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| created                    | string               | Creation date.                                                                                                                                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| setup                      | boolean              | This is read-only. It indicates that the ElasticBox appliance is set up and ready for use.                                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| administrators             | array                | List of users who can administer the organization.                                                                                                                                      |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| domains                    | string               | Domains that are a part of the organization.                                                                                                                                            |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| authentication             | object               | List of the authentication methods to allow single sign-on in the organization. Contains the following properties:                                                                      |
|                            |                      |  * github: Boolean. If enabled, it is true, else false.                                                                                                                                 |
|                            |                      |  * google: Boolean. If enabled, it is true, else false.                                                                                                                                 |
|                            |                      |  * password: Boolean. If enabled, it is true, else false.                                                                                                                               |
|                            |                      |  * ldap: Boolean. If enabled, it is true, else false.                                                                                                                                   |
|                            |                      |  * ldap_config: Object that contains the LDAP service settings:                                                                                                                         |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * ldap_group_sync: Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                         |
|                            |                      |    * sources: Array of `LDAP sources </../documentation/managing-your-organization/user-authentication/#userauth-ldap-setup>`_. Each source has the following properties:               |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * host: Required. String identifies the hostname or IP address of the LDAP service.                                                                                              |
|                            |                      |        * groups_dn: String specifies a fully qualified group name.                                                                                                                      |
|                            |                      |        * group_dn_filter: String defines an entity on the LDAP server. All groups are synchronised as children of this entity.                                                          |
|                            |                      |        * email_field: String specifies the email field name by which to look up users. Typically, this field is called email.                                                           |
|                            |                      |        * ldap_search_password: String specifies the password for the LDAP service account to look up users who try to log in.                                                           |
|                            |                      |        * ldap_search_user: String specifies the username of the LDAP service account to look up users who try to log in.                                                                |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ldap_last_sync_completed   | string               | Timestamp of the last successful LDAP group sync, for example, 2015-04-06 14:28:12.874910. Value is null if ldap_group_sync is set to false.                                            |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ldap_groups                | array                | List of objects, each of which is an LDAP group. Each group has two properties:                                                                                                         |
|                            |                      |  * dn: String identifier for the group.                                                                                                                                                 |
|                            |                      |  * name: String name shown in the workspace web interface.                                                                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| providers                  | array                | List of cloud providers the organization can enable to register and deploy. Each provider type has the following properties:                                                            |
|                            |                      |  * enabled: Boolean value of true if enabled, else false.                                                                                                                               |
|                            |                      |  * type: String values of the supported cloud providers: Amazon Web Services, Openstack, VMWare vSphere, Google Compute, Microsoft Azure, Cloudstack, SoftLayer, VMware vCloud Director,|
|                            |                      |    Amazon Web Services GovCloud, Rackspace.                                                                                                                                             |
|                            |                      |  * description: String that briefly enumerates the services from the cloud provider.                                                                                                    |
|                            |                      |  * pricing: Array of pricing information for Linux and Windows compute instance types. Only available for Amazon Web Services.                                                          |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| tags                       | array                | List of `tags </../documentation/managing-your-organization/resource-tags/#preset>`_ applied on instances deployed to cloud providers from the organization. Each tag has three         |
|                            |                      | properties:                                                                                                                                                                             |
|                            |                      |  * name: String you apply as a tag.                                                                                                                                                     |
|                            |                      |  * type: String identifies the type of tag whether an ElasticBox object or a custom one. Allowed values are Box, Workspace, Provider, Environment, Email, User ID, Service Instance ID, |
|                            |                      |    Service ID, Workspace ID, Instance ID, Custom.                                                                                                                                       |
|                            |                      |  * value: String value of null for ElasticBox objects. For custom tags, set its value using this property.                                                                              |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| webhooks                   | array                | List of webhooks that integrate with the organization.                                                                                                                                  |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| cost_centers               | array                | List of cost centers. Each cost center contains the following properties:                                                                                                               |
|                            |                      |  * enforce: Boolean. If true, an instance cannot be deployed if it is over the quota.                                                                                                   |
|                            |                      |  * name: String. Name of the cost center                                                                                                                                                |
|                            |                      |  * workspaces: Array. List of the names that belongs to the cost center.                                                                                                                |
|                            |                      |  * quotas: List of quotas. Each quota contains an object with the following properties:                                                                                                 |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * cost: Required. Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                          |
|                            |                      |    * provider: Required. Boolean. By default it's false. Specify as true to enable synchronizing with LDAP groups.                                                                      |
|                            |                      |    * allocated: Array. List of instances which are contributing to the current quota. Each allocated instance has these properties:                                                     |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * instance_id: Required. String. Id of the instance.                                                                                                                             |
|                            |                      |        * instances: Required. Int. Number of instances.                                                                                                                                 |
|                            |                      |        * started: Required. String. Date when this instance was deployed.                                                                                                               |
|                            |                      |        * flavor: Required. String. Type of instance.                                                                                                                                    |
|                            |                      |        * region: Required. String. Region where it was deployed.                                                                                                                        |
|                            |                      |        * service_type: Required. String. Type of the service.                                                                                                                           |
|                            |                      |        * terminated: String specifies the username of the LDAP service account to look up users who try to log in.                                                                      |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |    * resources: Object. Resources of the quota.                                                                                                                                         |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * cpu: Required. Int. Number of cpu units.                                                                                                                                       |
|                            |                      |        * disk: Required. Object. A disk with these properties:                                                                                                                          |
|                            |                      |           * quantity: Required. String. Amount of storage.                                                                                                                              |
|                            |                      |           * unit: Required. String. Mb, Gb or Tb.                                                                                                                                       |
|                            |                      |                                                                                                                                                                                         |
|                            |                      |        * ram: Required. String. Ram of the quota.                                                                                                                                       |
|                            |                      |           * quantity: Required. String. Amount of storage.                                                                                                                              |
|                            |                      |           * unit: Required. String. Mb or Gb.                                                                                                                                           |
+----------------------------+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Response Body**

.. raw:: html

	<pre>
	{
	   "schema":"http://elasticbox.net/schemas/organization",
	   "name":"organization_name",
	   "icon":null,
	   "theme":null,
	   "updated":"2015-04-06 16:59:02.486606",
	   "created":"2015-03-25 10:41:15.098256",
	   "setup":true,
	   "administrators":[
	      "operations"
	   ],
	   "domains":[
	      "elasticbox.com"
	   ],
	   "authentication":{
	      "ldap_config":{
	         "ldap_group_sync":false,
	         "sources":[
	            {
	               "host":"ldap://test_ldap"
	            }
	         ]
	      },
	      "github":true,
	      "google":true,
	      "ldap":true,
	      "password":true,
	   },
	   "ldap_groups":[

	   ],
	   "providers":[
	      {
	         "enabled":true,
	         "type":"Amazon Web Services",
	         "description":"Manage EC2, S3, Dynamo DB, RDS, ElastiCache and CloudFormation instances.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Google Compute",
	         "description":"Manage cloud hosting and Linux machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Openstack",
	         "description":"Manage cloud hosting, Linux and Windows machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"VMware vSphere",
	         "description":"Manage cloud hosting, Linux and Windows machines",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Microsoft Azure",
	         "description":"Manage compute services for Windows and Linux machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Cloudstack",
	         "description":"Manage cloud hosting, Linux and Windows machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"VMware vCloud Director",
	         "description":"Manage cloud hosting, Linux and Windows machines.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Amazon Web Services GovCloud",
	         "description":"Manage compute services in an isolated ITAR compliant AWS region.",
	         "pricing":[

	         ]
	      },
	      {
	         "enabled":true,
	         "type":"Rackspace",
	         "description":"Manage cloud hosting and Linux machines.",
	         "pricing":[

	         ]
	      }
	   ],
	   "ldap_last_sync_completed":null,
	   "tags":[
	      {
	         "name":"box",
	         "type":"Box name",
	         "value":null
	      },
	      {
	         "name":"environment",
	         "type":"Environment",
	         "value":null
	      },
	      {
	         "name":"devenv",
	         "type":"Custom",
	         "value":"ElasticBox Dev Environment"
	      }
	   ],
	   "cost_centers":[
	      {
	         "name":"test",
	         "enforce":false,
	         "quotas":[
	            {
	               "allocated":[

	               ],
	               "cost":0,
	               "provider":"2bf1bd2c-b03d-460f-80da-647d26bdbcfe"
	            },
	            {
	               "cost":3000,
	               "provider":"5908ee9b-0c0a-4af6-8eef-2dc9f95d033a"
	            }
	         ],
	         "workspaces":[
	            "operations"
	         ]
	      }
	   ],
	   "webhooks":[

	   ]
	}
	</pre>
