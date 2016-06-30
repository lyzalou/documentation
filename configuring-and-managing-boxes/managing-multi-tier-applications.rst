Binding Large Scale Deployments
***********************************

Bindings glue together different parts of a multi-tier application over the network. These components can be parts of an application stack, a load balancing pool, cluster, and more. Bindings enable services to auto-discover and connect dynamically at scale. They also reconfigure the instances that need it to keep their configuration up to date.

**In this article:**

* `What are bindings?`_
* `Which instances does a binding connect to?`_
* `How to use the bindings in configuration?`_
* `Bindings automatic reconfiguration`_
* `Using bindings in three steps`_
* `Step 1. Define binding variables`_
* `Step 2. Configure bindings for your application`_
* `Step 3. Tag bindings for instance connectivity`_

What are bindings?
----------------------

Bindings are `variables </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-bindingtype>`_ that you can add to boxes. They represent a connection from the deployed box to other instances. Bindings are used to configure your event scripts of configuration files to work with the other instances.

Which instances does a binding connect to?
----------------------------------------------

The instances where a binding variable points to are defined by the criteria you configure. You refine the criteria by setting a specific box type for the binding and specifying tags to match. A target instance will be in the binding of the origin instance if:

* The instance is shared in the workspace owner of the origin instance.
* It has connection information available, this usually means that the addresses are available.
* The target instance is not terminated.
* The target instance contains a box of the type in the binding variable.
* The target instance contains at least all the tags in the binding variable.

Here is how the binded instances show after the instance is online.

.. raw:: html

    <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/bindings/bindings-list.png" alt="List of bindings of an instance">
    </div>

How to use the bindings in configuration?
---------------------------------------------

The bindings information is provided in the event scripts (except the install scripts) and configuration files (via elasticbox conf). A binding is a jinja2 list which contains many information of the target instance to be used. The most commonly used information are addresses and variables.

To access a variable on the box of the target instance, just use it’s variable name. For example binding[0].varName access to the varName variable of the first instance in the binding.

To access the addresses of the target instance, you can use binding[1].address.private or binding[1].public. This allows you to access the private address or, if it’s not available, the public one.

To do something for each instance in the binding, you can use a jinja2 loop. For example, to print an IP per binding instance you can use:

.. raw:: html

	<pre>
	{% for i in binding %}
	{{ i.address.public or i.address.private }}
	{% endfor %}
	</pre>

Lastly, the list of bindings provides direct access to the first instance attributes. For example,

.. raw:: html

	<pre>
	{{ binding.varName }}
	</pre>

is equivalent to

.. raw:: html

	<pre>
	{{ binding[0].varName }}
	</pre>

Bindings automatic reconfiguration
-------------------------------------

The bindings are calculated after any install event and before the reconfiguration. At that point all the matching instances are saved and will be available in the different events. Keeping the bindings up to date implies reconfiguring the instances to update the bindings and all configuration scripts and files. To help with this task, ElasticBox automatically reconfigures the instances that require it after some changes. You can trust ElasticBox to keep updated the following information:

* The target instances in each binding. This means deploying or terminating instances, or changing their tags, will automatically reconfigure the instances that require it to keep the information up to date.
* The IP information shown by ElasticBox. Every time we detect an IP change, the instances that point to that instance will reconfigure. The detection of IP is provider specific.

This covers almost all cases of reconfiguration, but there are two details to take into consideration. First, we don’t reconfigure if a variable is changed via elasticbox.set or with the lifecycle editor. If other instances are accessing that variable through bindings, you need to reconfigure them also. Second, after a pull version on an instance you should reconfigure all instances that point to that instance so that they get the latest version of the box variables.

Using bindings in three steps
-------------------------------------

There are three steps to make bindings work. Say a Node.js application needs MongoDB and needs to become a member of the frontend Nginx load balancing pool. To enable the database and load balancer connections in this example, we use bindings.

Step 1. Define Binding Variables
------------------------------------

Binding variables are `defined in box automation </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-bindingtype>`_. In this example, we defined two binding variables. One in the Nginx loadbalancer box to connect to Node.js application instances, and another in the Node.js application box to connect to the MongoDB database.

In both cases, the bindings point to a box type, which allow the services to bind only to instances of the box type at deploy time.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/bindings/example-Nginx-binding-to-Nodejsapp-box.png" alt="Binding to Connect Nginx to Node.js App Instances">
	</div>

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/bindings/example-Nodejs-binding-to-Mongodb-box.png" alt="Binding to Connect Node.js Instances to a MongoDB Database">
	</div>

Step 2. Configure Bindings for Your Application
---------------------------------------------------

To establish connectivity to a remote service, we must configure the bindings in the box. We do so in the box configure script. Here's an example. To connect the Nginx loadbalancer to the Node.js application instances, we configure the bindings as follows. In the Nginx loadbalancer box configure script, we run the ElasticBox config command to execute a file variable (ngix.conf)that has the connection string.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/bindings/example-nginx-configurebindings-1.png" alt="Configure Binding in Nginx Box">
	</div>

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/bindings/example-nginx-configurebindings-2.png" alt="Configure Binding in Nginx Box">
	</div>

Here the ‘services’ binding variable polls each instance fulfilling the binding criteria. As you know, when we defined the binding, the criteria was to connect to all instances of the Node.js application box type.

.. raw:: html

	<pre>
	upstream services {
    	{% for binding in services %}
            server {{ binding.address.public or binding.address.private }}:{{ binding.PORT }};
    	{% endfor %}
	}
	</pre>

Within the connection string, each element polls values of the Node.js application box variables, like the port. IP addresses are `default system variables </../documentation/configuring-and-managing-boxes/syntax-for-variables/#syntax-default-variables>`_ available for every instance. Besides IP addresses, bindings provide lots of helpful data about remote services. You can pretty much query ports or any other variables defined in box automation through bindings.

Step 3. Tag Bindings for Instance Connectivity
--------------------------------------------------

Tagging bindings allow services to discover each other automatically. At deploy time, for the bindings defined in the boxes, we need to apply tags of instances to which they can bind. In this example, we ask the Nginx loadbalancer to bind to instances tagged production and nodejsapp. What this does is, the binding not only looks for instances of a particular box type as defined in the box but makes sure the instances match these tags. The binding takes effect only when both conditions are met. This is how tagged bindings allow instances to connect automatically to one or many services at scale.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/bindings/example-tag-binding-deploytime-nginx.png" alt="Tag Binding to Node.js Instances in Nginx Box at Deploy Time">
	</div>

As another example, here we launch the Node.js application specifying its binding connection to MongoDB instances tagged production, mongodb. Importantly, also note that we tagged the Node.js instance as production and nodejsapp so that Nginx can bind to it.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/bindings/example-tag-binding-deploytime-nodejsapp.png" alt="Tag Binding to MongoDB Instances in Node.js Box at Deploy Time">
	</div>











