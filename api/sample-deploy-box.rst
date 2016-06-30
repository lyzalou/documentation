Using the API: from registering a provider to the deployment of an instance
*******************************************************************************

This guide shows all the steps you need to deploy a box using the ElasticBox API.

* `Authentication and deployment variables`_
* `Register a provider`_
* `Create a new Deployment Policy Box`_
* `Create a Box`_
* `Deploy the instance`_
* `Terminate the instance`_

**Note**: We use cURL commands to send HTTP requests to the API objects in JSON. JSON is the required format for all API requests and responses.

Now let's look at the script in sections to understand how you can make API calls from any code you like.

.. _Authentication and deployment variables:

Authenticate with ElasticBox
------------------------------------

Before calling to the API you have to sig in into the ElasticBox website and `getting an authentication token </../documentation/api/overview-access/#api-get-token>`_. You use this token as an http header to perform every call to the Elasticbox API.

Declare Deployment Variables
````````````````````````````````

In this case we decided to use AWS so we will need the account key and secret to register it as provider. Box deployment arguments such as owner, environment and token are declared as variables because they can differ between deployments. That way, each time you run the script, you can pass different arguments.

.. raw:: html

	<pre>
	aws_provider_key=$1
	aws_provider_secret=$2
	environment=$3
	elasticBox_token=$4
	owner=$5
	</pre>

.. _Register a provider:

Register a Provider in ElasticBox
-------------------------------------

A provider is a public or private cloud account you can register in ElasticBox. In this case we choose AWS as provider. From the AWS account credential we will need the key and the secret right now

.. raw:: html

	<pre>
	# Register the provider in Elastic Box
	payload="{
	  \"icon\": \"images/platform/aws.png\",
	  \"type\": \"Amazon Web Services\",
	  \"description\": \"Manage EC2, ECS, S3, Dynamo DB, RDS, ElasticCache, and CloudFormation instances\",
	  \"schema\": \"http://elasticbox.net/schemas/aws/provider\",
	  \"name\": \"AWS Example Provider via CURL\",
	  \"credentials\": {
	    \"key\": \"$example_provider_key\",
	    \"secret\": \"$example_provider_secret\"
	  },
	  \"owner\": \"$owner\"
	}"
	provider=$(curl -k -s \
	    -X POST \
	    -H "elasticBox-token:$elasticBox_token" \
	    -H "elasticbox-release:4.0" -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/534.30 (KHTML, like Gecko) Chrome/12.0.742.112 Safari/534.30" \
	    -H "Accept: application/json, text/plain, */*" \
	    -H "Accept-encoding: gzip, deflate" \
	    -H "Content-Type: application/json" \
	    -d "$payload" https://$environment/services/providers)
	provider_id=$(echo $provider | python -c 'import json,sys; print json.load(sys.stdin)["id"]')
	if [ -z != $provider_id ]; then
	    echo "Created provider $provider_id"
	else
	    echo "Error creating the provider: $provider"
	fi;
	</pre>

Create a new Deployment Policy Box
-------------------------------------

A deployment policy box is where you specify settings to deploy applications in a specific virtual environment. We send parameters for creating the deployment policy box in a POST request to the box object. If there’s an error in creating the policy box, we output the error. Else, we output the created box.

.. raw:: html

	<pre>
	# Once we have created the provider we can set the proper policy box
	payload="{
	  \"owner\": \"$owner\",
	  \"schema\": \"http://elasticbox.net/schemas/boxes/policy\",
	  \"claims\": [
	    \"linux\"
	  ],
	  \"lifespan\": {
	    \"operation\": \"none\"
	  },
	  \"automatic_updates\": \"off\",
	  \"provider_id\": \"$provider_id\",
	  \"name\": \"Deployment Policy Box Example\",
	  \"description\": \"Just one example of deployment policy box creation via API\",
	  \"profile\": {
	    \"schema\": \"http://elasticbox.net/schemas/aws/ec2/profile\",
	    \"flavor\": \"t1.micro\",
	    \"location\": \"us-east-1\",
	    \"instances\": 1,
	    \"image\": \"Linux Compute\",
	    \"keypair\": \"None\",
	    \"cloud\": \"EC2\",
	    \"security_groups\": [

	    ],
	    \"subnet\": \"us-east-1a\"
	  }
	}"
	policy_box=$(curl -k -s \
	    -X POST \
	    -H "Content-Type:application/json" \
	    -H "elasticBox-token:$elasticBox_token" \
	    -H "elasticbox-release:4.0" -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/534.30 (KHTML, like Gecko) Chrome/12.0.742.112 Safari/534.30" \
	    -H "Accept: application/json, text/plain, */*" \
	    -H "Accept-encoding: gzip, deflate" \
	    -H "Content-Type: application/json;charset=UTF-8" \
	    -d "$payload" https://$environment/services/boxes)
	policy_box_id=$(echo $policy_box | python -c 'import json,sys; print json.load(sys.stdin)["id"]')
	if [ -z != $policy_box_id ]; then
	    echo "Created policy_box $policy_box_id"
	else
	    echo "Error launching the policy_box: $policy_box"
	fi;
	</pre>

.. _Create a Box:

Create a custom box in order to deploy it using the previous deployment policy box
--------------------------------------------------------------------------------------

In a POST request to the Box object, we send the features for our script box. Also we will use two already created blobs. You can take a look to the `blobs </../documentation/api/blobs/>`_, API doc if you need to know how you can create them

.. raw:: html

	<pre>
	# We create a box to deploy with that policy box, in this case we are going to use a previously uploaded scripts.
	payload="{
	  \"automatic_updates\": \"off\",
	  \"requirements\": [

	  ],
	  \"description\": \"sample box created via API\",
	  \"name\": \"ScriptBoxSample\",
	  \"deleted\": null,
	  \"variables\": [
	    {
	      \"required\": true,
	      \"type\": \"Text\",
	      \"name\": \"variable_name\",
	      \"value\": \"variable_value\",
	      \"visibility\": \"public\"
	    }
	  ],
	  \"visibility\": \"workspace\",
	  \"members\": [

	  ],
	  \"owner\": \"$owner\",
	  \"organization\": \"elasticbox\",
	  \"events\": {
	    \"configure\": {
	      \"url\": \"/services/blobs/download/5631fa7614841250525226cc/configure\",
	      \"length\": 5,
	      \"destination_path\": \"scripts\",
	      \"content_type\": \"text/x-shellscript\"
	    },
	    \"install\": {
	      \"url\": \"/services/blobs/download/5631fa6614841250525226ca/install\",
	      \"length\": 5,
	      \"destination_path\": \"scripts\",
	      \"content_type\": \"text/x-shellscript\"
	    }
	  },
	  \"schema\": \"http://elasticbox.net/schemas/boxes/script\"
	}"
	box=$(curl -k -s \
	    -X POST \
	    -H "Content-Type:application/json" \
	    -H "elasticBox-token:$elasticBox_token" \
	    -H "elasticbox-release:4.0" -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/534.30 (KHTML, like Gecko) Chrome/12.0.742.112 Safari/534.30" \
	    -H "Accept: application/json, text/plain, */*" \
	    -H "Accept-encoding: gzip, deflate" \
	    -H "Content-Type: application/json;charset=UTF-8" \
	    -d "$payload" https://$environment/services/boxes) \
	box_id=$(echo $box | python -c 'import json,sys; print json.load(sys.stdin)["id"]')
	if [ -z != $box_id ]; then
	    echo "Created box $box_id"
	else
	    echo "Error launching the box: $box"
	fi;
	</pre>

Deploy the Instance
-----------------------

To remove the instance from the virtual machine, we send a DELETE request to the Instances object with the instance ID. Then we check its response status. If it's 200, we say that the specific instance is terminated. Else, we output the error state from the response.

.. raw:: html

	<pre>
	# Finally we are going to deploy the instance
	payload="{
	    \"schema\": \"http://elasticbox.net/schemas/deploy-instance-request\",
	    \"owner\": \"$owner\",
	    \"name\": \"ScriptBoxSample\",
	    \"box\": {
	        \"id\": \"$box_id\",
	        \"variables\": [
	        ]
	    },
	    \"instance_tags\": [

	    ],
	    \"automatic_updates\": \"off\",
	    \"policy_box\": {
	        \"id\": \"$policy_box_id\",
	        \"variables\": [

	        ]
	     }
	}"
	instance=$(curl -k -s \
	    -X POST \
	    -H "Content-Type:application/json" \
	    -H "elasticBox-token:$elasticBox_token" \
	    -H "elasticbox-release:4.0" -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/534.30 (KHTML, like Gecko) Chrome/12.0.742.112 Safari/534.30" \
	    -H "Accept: application/json, text/plain, */*" \
	    -H "Accept-encoding: gzip, deflate" \
	    -H "Content-Type: application/json;charset=UTF-8" \
	    -d "$payload" https://$environment/services/instances)
	instance_id=$(echo $instance | python -c 'import json,sys; print json.load(sys.stdin)["id"]')
	if [ -z != $instance_id ]; then
	    echo "Deployed instance $instance_id"
	else
	    echo "Error deploying the instance: $instance_id"
	fi;
	</pre>

Terminate the instance
---------------------------

To remove the instance from the virtual machine, we send a DELETE request to the Instances object with the instance ID. Then we check its response status. If it's 200, we say that the specific instance is terminated. Else, we output the error state from the response.

.. raw:: html

	<pre>
	curl -k -s \
	-X DELETE \
	-H "elasticBox-token:$elasticBox_token" \
	-H "elasticbox-release:4.0" \
	-A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/534.30 (KHTML, like Gecko) Chrome/12.0.742.112 Safari/534.30" \
	-H "Accept: application/json, text/plain, */*" \
	-H "Accept-encoding: gzip, deflate" \
	 https://$environment/services/instances/$instance_id

	echo "Undeployed box: $instance_id"
	</pre>




