Boxes
*****

Boxes are the templates that store application automation. An instance is a box you install on virtual infrastructure provisioned to a public, private cloud provider, or your own infrastructure. Take a `quick tour </../../documentation/getting-started/login-and-quick-tour/#tour>`_ to understand the layout of boxes and instances in ElasticBox.

Boxes contain scripts, variables, and metadata to automate processes when instantiated on cloud infrastructure. Stitched together, boxes model complex processes like deploying or upgrading multi-tier enterprise scale applications.

So how do boxes work? A typical application stack may consist of multiple boxes, each one modeling a step of the application's install. For example, one might model the install of runtime requirements (such as PHP libraries). Another might model the install of a web server (such as Apache). And a third might model connecting to a source control repository (such as Git), pulling the latest code, and installing it on the virtual server. When stacked and instantiated these three boxes install an application. At the same time, each box is independent, reusable and can be consumed by other applications.

**In this article:**

* `Understanding box basics`_

Understanding Box Basics
------------------------

New Box
```````

To create a new one, click **New**. Select a box type to match your automation:

* `Script </../documentation/configuring-and-managing-boxes/script-box/>`_. To automate using Bash, PowerShell, Salt, Ansible, Puppet, or Chef.

* `Deployment Policy </../documentation/configuring-and-managing-boxes/deploymentpolicy-box/>`_. To select and share infrastructure resources, networking, and more from a cloud provider.

* `Application </../documentation/configuring-and-managing-boxes/application-box/>`_. To configure several boxes to deploy an application with a single click.

* `Template </../documentation/configuring-and-managing-boxes/template-box/>`_. To automate using AWS CloudFormation templates or ARM templates.

* `Container </../documentation/configuring-and-managing-boxes/docker-container-service/>`_. To automate using container technology like Docker.

Give it a name, optionally a description, and define some basic metadata:

+------------------+-----------------+---------------------------------------------------------+
| Metadata         | Box Type        | What it means                                           |
+==================+=================+=========================================================+
|Requirements      | Script,         | It's good practice to tag the runtime that the box      |
|                  | Template,       | requires to deploy. ElasticBox auto suggests tags like  |
|                  | Container       | Linux, Ubuntu, Java and so on. When ready to launch     |
|                  |                 | the box, you are presented with deployment policies     |
|                  |                 | that match the requirements. These deployment           |
|                  |                 | policies provide the right infrastructure or services   |
|                  |                 | the box needs to deploy.                                |
|                  |                 |                                                         | 
|                  |                 | **Note**: In CloudFormation boxes, the tags help look   |	
|                  |                 | instances for bindings.                                 |
+------------------+-----------------+---------------------------------------------------------+
|Automatic         | Script,         | Select the level of updates to automatically apply to   |
|Updates           | Template,       | instances you launch of a box version:                  |
|                  | Container       |                                                         |
|                  |                 | * **Off**. It's turned off by default.                  |
|                  |                 | * **All Updates**. Applies all changes.                 |
|                  |                 | * **Minor & Patch Updates**. Applies minor and patch    |
|                  |                 |   changes to the version deployed.                      |
|                  |                 | * **Patch Updates**. Applies only the patch changes to  |
|                  |                 | 	 the version deployed.                                 |
|                  |                 |                                                         |
+------------------+-----------------+---------------------------------------------------------+
|Provider          | Deployment      | Select the cloud provider registered in ElasticBox for  |
|                  | Policy          | which you will carve out infrastructrure resources in   |
|                  |                 | the policy.                                             |
+------------------+-----------------+---------------------------------------------------------+
|Claims            | Deployment      | Tag the services and infrastructure that a policy       |
|                  | Policy          | provides like Linux, Ubuntu 12.04, load balancing, and  |
|                  |                 | so on for deployments. Add claims so that boxes with    |
|                  |                 | matching requirements can successfully deploy using the |
|                  |                 | right policy.                                           |
+------------------+-----------------+---------------------------------------------------------+




Box Sections
````````````

Once you create a box, you can configure and manage it in these sections.

.. raw:: html

	<div class="doc-image padding-1x">
    	<div class="browser-feature">
        	<div class="indicators">
            	<div class="circle magenta"></div>
            	<div class="circle orange"></div>
            	<div class="circle green"></div>
          	</div>
          	<div class="browser-window">
            	<img class="img-responsive" src="/../assets/img/docs/boxes/box-box.png" alt="Box Sections">
          	</div>
      	</div>
    </div>

+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
| Section            | Description                                                                                                                                     |
+====================+=================================================================================================================================================+
| Configuration      | Automate how a piece of software deploys in the virtual environment by                                                                          |
|                    | by parameterizing with `variables </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/>`_                     |
|                    | and `events </../documentation/configuring-and-managing-boxes/start-stop-and-upgrade-boxes/>`_.                                                 |
+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+ 
| Versions           | Keep track deployment configuration changes with the help of versioning. Versions let you consume different configurations                      |
|                    | of the same box in multiple deployments. From this tab, you can create a new version, see a diff of what changed, or restore                    |
|                    | a version as the box draft.                                                                                                                     | 
+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
| Share              | Invite team members to `collaborate </../documentation/core-concepts/workspaces-and-collaboration/>`_ and improve the                           |
|                    | configuration or just let them deploy the box.                                                                                                  |
+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
| Deploy             | `Launch a new instance </../documentation/deploying-and-managing-instances/deploying-managing-instances/>`_ of the box draft with this option.  |
|                    | This lets you select a specific deployment policy to launch on a cloud provider.                                                                |
+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
| Gear Menu          | From here, you can edit basic metadata of the box or delete it.                                                                                 |
+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+

