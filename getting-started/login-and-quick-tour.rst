Login and Quick Tour
********************************

**In this article:**

* `Login`_
* `Quick tour`_

Login
----------------------------------

**Where to log in?**

* To access ElasticBox in the cloud, browse to http://elasticbox.com/login or your ElasticBox company URL.
* To access ElasticBox inside a private network, ask your administrator for the login URL.
	**Note**: Enable JavaScript on the browser to use ElasticBox. If not, some elements of the web interface may not work or render properly. We support ElasticBox on the latest Chrome, Safari, Firefox, and Internet Explorer browsers.

**How to log in?**

You have a few options:

* `Sign up <http://elasticbox.com/signup>`_ for an ElasticBox account with a username and password.
* `Log in <http://elasticbox.com/login>`_ with an existing Google account.
* Log in with your current GitHub credentials if your org admin has enabled GitHub signin for ElasticBox.
* Enter your company Active Directory credentials in the username, password fields if your admin enabled LDAP integration with ElasticBox.

**Note:** When you log in with your AD credentials, GitHub or Google accounts, ElasticBox does not have access to your password. We use your email to create a profile and workspace for you.

.. raw:: html

	<div class="doc-image">
		<div class="browser-feature">
	    	<div class="indicators">
	        	<div class="circle magenta"></div>
	        	<div class="circle orange"></div>
	        	<div class="circle green"></div>
	      	</div>
	      	<div class="browser-window">
	        	<img class="img-responsive" src="/../assets/img/docs/gettingstarted/login.png" alt="ElasticBox Login">
	      	</div>
	  	</div>
	</div>

Quick Tour
----------------------------------

When you log in, the top navigation bar guides you to the main areas of ElasticBox.

* `Workspaces`_
* `Instances`_
* `Boxes`_
* `Providers`_
* `Your Account`_
* `Help`_

Workspaces
===========================

.. raw:: html

	<div class="doc-image padding-1x">
		<div class="browser-feature">
		    <div class="indicators">
		        <div class="circle magenta"></div>
		        <div class="circle orange"></div>
		        <div class="circle green"></div>
		    </div>
		    <div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/gettingstarted/quicktour/quicktour-workspaces.png" alt="Take a Quick Tour">
		    </div>
	  	</div>
	</div>

When you log in, you're in your personal workspace. From the workspace drop-down, you can access team workspaces if you belong to them, or create one and add others to share ElasticBox assets and collaborate. To learn more, see `Workspaces & Sharing </../documentation/core-concepts/workspaces-and-collaboration/>`_.

Instances
===========================

.. raw:: html

	<div class="doc-image padding-1x">
		<div class="browser-feature">
			<div class="indicators">
		    	<div class="circle magenta"></div>
		        <div class="circle orange"></div>
		        <div class="circle green"></div>
		    </div>
		    <div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/gettingstarted/quicktour/quicktour-instances.png" alt="Take a Quick Tour">
		    </div>
		</div>
	</div>

* Click **New Instance** to launch instances of boxes to a provider environment in the public or private cloud.
* Manage instances you've `launched </../documentation/deploying-and-managing-instances/deploying-managing-instances/>`_ through the web interface, the API, or `on any infrastructure using the ElasticBox agent </../documentation/deploying-and-managing-instances/deploying-on-anyinfra/>`_. Here you can quickly `manage the lifecycle </../documentation/deploying-and-managing-instances/deploying-managing-instances/#actions>`_ of several instances from the Bulk Actions menu or handle them individually from the gear menu.
* Find an instance by searching any part of its name, or click filtered views of instances you launched or that were shared with you. Or locate them by tags.

Boxes
===========================

The Boxes page shows everything you create including boxes shared with you.

.. raw:: html

	<div class="doc-image padding-1x">
    	<div class="browser-feature">
        	<div class="indicators">
            	<div class="circle magenta"></div>
            	<div class="circle orange"></div>
            	<div class="circle green"></div>
          	</div>
          	<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/gettingstarted/quicktour/quicktour-boxes.png" alt="Take a Quick Tour">
          	</div>
      	</div>
	</div>

* Click **New** to create a box. Automate configuration by selecting a box type: Script, Deployment Policy, CloudFormation, Container.
* ElasticBox provides a public service box catalog, which you find when you filter by the public tag. Public boxes are available to all users. These are pre-configured, which means you can directly deploy or nest them in other boxes. Since they're publicly available to everyone, you can't modify their configuration, but you can pass your own parameters before deploying. Examples of public boxes include MongoDB, Puppet, Chef Solo, Rails, Redis, RabbitMQ, WordPress among others.
	**Note**: Some public boxes require an Internet connection to install software. So if you're using ElasticBox as an appliance in your private datacenter without Internet access, these boxes will not deploy.
* Organize boxes as icons or as a list. Sort alphabetically by name, most recently viewed, or by the owner.
* Search for a box by name, owner, or filter by technology under Requirements. The owner is a user or a workspace.

Providers
===========================

.. raw:: html

	<div class="doc-image padding-1x">
    	<div class="browser-feature">
        	<div class="indicators">
            	<div class="circle magenta"></div>
            	<div class="circle orange"></div>
            	<div class="circle green"></div>
          	</div>
          	<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/gettingstarted/quicktour/quicktour-providers.png" alt="Take a Quick Tour">
          	</div>
      	</div>
	</div>

* Connect to a provider to orchestrate deployments. Click **New Provider** to add AWS, Azure, vSphere, Google Cloud, OpenStack, or CloudStack.
* Locate a provider through search or by type.
* Sync or delete provider accounts using Bulk Actions.

Your Account
===========================

.. raw:: html

	<div class="doc-image padding-1x">
    	<div class="browser-feature">
        	<div class="indicators">
            	<div class="circle magenta"></div>
            	<div class="circle orange"></div>
            	<div class="circle green"></div>
          	</div>
          	<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/gettingstarted/quicktour/quicktour-youraccount.png" alt="Take a Quick Tour">
          	</div>
      	</div>
	</div>

* From the username drop-down, access your account profile where you can change your username or reset password if you're using username and password to log in.
* Access the Admin Console from here to administer your ElasticBox organization.
* Get tokens to use ElasticBox via the CLI or API with Authentication Tokens.

Help
===========================

Get help from `docs </../documentation/>`_ or contact `support`_.

.. _support: support@elasticbox.com

.. raw:: html

	<div class="doc-image padding-1x">
    	<div class="browser-feature">
        	<div class="indicators">
            	<div class="circle magenta"></div>
            	<div class="circle orange"></div>
            	<div class="circle green"></div>
          	</div>
          	<div class="browser-window">
				<img class="img-responsive" src="/../assets/img/docs/gettingstarted/quicktour/quicktour-help.png" alt="Take a Quick Tour">
          	</div>
      	</div>
	</div>








