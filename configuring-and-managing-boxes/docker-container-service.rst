Deploying to Docker Containers from ElasticBox
*************************************************

Container boxes enable you to define and deploy `Docker <https://docs.docker.com/introduction/understanding-docker/>`_ images in Linux environments. The advantage of deploying to Docker containers through ElasticBox is to be able to consume Dockerfiles directly in boxes. Secondly, you can bind together distributed, multi-tier applications that involve automation in boxes and Docker containers.

**In this article:**

* `How Docker works in ElasticBox`_

* `Configuring and Deploying Docker Containers`_

* `Limitations with using Docker`_

* `Managing the lifecycle of Docker containers`_

How Docker Works in ElasticBox
---------------------------------

Docker works in ElasticBox through the Docker Container service box, a Linux box that understands Docker commands when deployed. In the box you model your application in a Dockerfile, which helps create the image of your application in a Docker container. Write Dockerfile commands not only in `Docker instructions <https://docs.docker.com/reference/builder/>`_, but also in Bash and PowerShell.

At deploy time, ElasticBox executes the Docker box like any other box in the virtual environment. ElasticBox treats the Docker container like a `box variable </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-boxtype>`_ and the Dockerfile as a `file variable </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-filetype>`_ inside it. On the virtual machine, ElasticBox first installs the Docker client from which we install the Docker container using the daemon BUILD command.

Configuring and Deploying Docker Containers
----------------------------------------------

See how deploying to Docker containers works in ElasticBox using `ElasticSearch <http://www.elasticsearch.org/overview/>`_ as an example. ElasticSearch is a RESTful search engine that makes it easy to analyze, explore, and visualize data. To get started, we create a container box and then configure the application in a Dockerfile.

Create a Docker Container box
````````````````````````````````

From the Boxes page, click **New** > **Container**. In the dialog, enter a name to identify it in the box service catalog. And optionally enter other `metadata </../documentation/core-concepts/boxes/#box-metadata>`_. Then save to continue.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-newbox.png" alt="Create a New Box Based on the Docker Container Service">
	</div>

Create a Dockerfile
```````````````````````

You now see a box layout where you can configure your application’s deployment with `variables </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/>`_ and the Dockerfile.

.. raw:: html

	<div class="doc-image padding-1x">
		<div class="browser-feature">
	        <div class="indicators">
	            <div class="circle magenta"></div>
	            <div class="circle orange"></div>
	            <div class="circle green"></div>
	        </div>
			<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-newbox-configurationlayout.png" alt="View the Docker Box Configuration Layout">
			</div>
		</div>
	</div>

Let's add a Dockerfile to construct the ElasticSearch image that will run in the Docker container. Under Dockerfile, click **New**. In the dialog, click **Create a Blank Dockerfile**. This creates an empty Dockerfile.

.. raw:: html

	<div class="doc-image padding-1x">
		<div class="browser-feature">
	        <div class="indicators">
	            <div class="circle magenta"></div>
	            <div class="circle orange"></div>
	            <div class="circle green"></div>
	        </div>
			<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-emptydockerfile.png" alt="Create a Blank Dockerfile">
			</div>
		</div>
	</div>

Edit to add instructions that install ElasticSearch.

.. raw:: html

	<pre>
	#
	# ElasticSearch Dockerfile
	#
	# https://github.com/dockerfile/elasticsearch
	#
	 
	# Pull base image.
	FROM dockerfile/java
	 
	# Install ElasticSearch.
	RUN \
	  cd /tmp && \
	  wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.2.1.tar.gz && \
	  tar xvzf elasticsearch-1.2.1.tar.gz && \
	  rm -f elasticsearch-1.2.1.tar.gz && \
	  mv /tmp/elasticsearch-1.2.1 /elasticsearch

	# Define mountable directories.
	VOLUME ["/data"]

	# Define working directory.
	WORKDIR /data

	# Define default command.
	CMD ["/elasticsearch/bin/elasticsearch"]
	</pre>

Instructions in the Dockerfile are straightforward: On top of a Linux base image that has Java, we download ElasticSearch, mount a drive, and install ElasticSearch in that location.

Specify Ports for the Docker Container
`````````````````````````````````````````

To allow traffic to and from ElasticSearch in the Docker container on its host, we need to specify ports. To do that we add port variables. Under Variables, click **New**. Here we create one to allow HTTP traffic through port 9200 and another for ElasticSearch to communicate internally over port 9300.

.. raw:: html

	<div class="doc-image padding-1x">
    	<div class="browser-feature">
        	<div class="indicators">
            	<div class="circle magenta"></div>
            	<div class="circle orange"></div>
            	<div class="circle green"></div>
          	</div>
			<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-addportvariables.png" alt="Add Port Variables to the Dockerfile">
			</div>
		</div>
	</div>

As soon as we add the variables, ElasticBox automatically generates an EXPOSE instruction for them in the Dockerfile. This tells the Docker container to listen on ports 9200 and 9300.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-EBautogenerates-exposeforports.png" alt="ElasticBox Auto Generates EXPOSE for Port Variables">
	</div>

Text, number, or options type variables are handled at deploy time as `Docker environment variables <http://docs.docker.com/reference/builder/#env>`_. Use this syntax to refer to them in Dockerfiles: {{variable_name}}

A file variable is handy to run additional commands using RUN or trigger an executable file using CMD. But first you must copy it from the ElasticBox remote URL to the container's filesystem at the path you specify using `ADD <http://docs.docker.com/reference/builder/#add>`_ in the Dockerfile: **ADD {{file_variable_name}} destination_path_in_container**

`Bindings </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-bindingtype>`_ pass connections strings or deployment values to connect with other Docker containers or boxes. To bind to another Docker container or box, create a binding and pass binding references via text expression variables with this syntax: **{{ binding_name.variable_name }}**

Here we connect to a box that deploys an S3 bucket using a binding. As Dockerfiles don't allow scripts, we use a text expression to pass the binding reference.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-createbinding.png" alt="Create a Binding">
	</div>

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-passbindingereference.png" alt="Pass Binding Reference via Text Expressions">
	</div>

Deploy the Docker Box
```````````````````````````

You can launch the Docker box in any environment including public, private clouds or a datacenter. Here we launch the ElasticSearch Docker box in AWS. In the ElasticSearch box page, click **Deploy**. Choose a deployment policy box that contains deployment settings for a cloud provider and optionally add tags, `auto schedule </../documentation/deploying-and-managing-instances/deploying-managing-instances/#instance-scheduler>`_ the container, and specify other `metadata </../documentation/core-concepts/boxes/#box-metadata>`_.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-selectdeploymentsettings.png" alt="Deploy a Docker Container">
	</div>

Expand ElasticSearch to see all the variables from the docker box. See how you can change deployment values before deploying. Click **Deploy** to launch the box.

Limitations with Using Docker
----------------------------------------------

Note these limitations when using Docker in ElasticBox.

* A docker box can install one image per container on an instance.
* Dockerfile Docker commands don't support arguments or flags. That said, you can pass arguments through the ENTRYPOINT command.
* When using the VOLUME command, you can store data in directories on the container, but can't mount or map them to the host filesystem.
* Deploy Docker using these supported Linux distributions: Ubuntu 15.04, Ubuntu 14.10, Ubuntu 14.04, Ubuntu 13.10, Ubuntu 13.04, Ubuntu 12.10, CentOS, Red Hat Enterprise Linux 6.5 or later, Fedora.

Managing the Lifecycle of Docker Containers
----------------------------------------------

You can manage the `lifecycle </../documentation/deploying-and-managing-instances/deploying-managing-instances/#actions>`_ of Docker containers like any other box. This means after deploying, you can change the Dockerfile configuration and relaunch it in the same instance using the instance `lifecycle editor </../documentation/core-concepts/lifecycle-editor/>`_. Go to the instance page and click **Lifecycle Editor**.

.. raw:: html

	<div class="doc-image padding-1x">
    	<div class="browser-feature">
        	<div class="indicators">
            	<div class="circle magenta"></div>
            	<div class="circle orange"></div>
            	<div class="circle green"></div>
          	</div>
			<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-launchlifecycleeditor.png" alt="Launch Lifecycle Editor">
			</div>
		</div>
	</div>

In the editor, you can edit the Dockerfile, variables, and use the actions drop-down to relaunch changes in the instance. To process these actions on the container in the backend, we run Docker daemon commands such as BUILD, RUN, KILL, and REMOVE.

.. raw:: html

	<div class="doc-image padding-1x">
    	<div class="browser-feature">
        	<div class="indicators">
            	<div class="circle magenta"></div>
            	<div class="circle orange"></div>
            	<div class="circle green"></div>
          	</div>
			<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/dockercontainer/dockercontainer-editdockerconfigurationforinstance.png" alt="Edit Docker Configuration in the Instance">
			</div>
		</div>
	</div>

Here’s what happens when you reinstall or reconfigure:

* Reinstall recreates the container on the instance.
* Reconfigure launches a new container replacing the existing one.

