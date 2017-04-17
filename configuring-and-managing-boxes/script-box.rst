Script Boxes
************

The Script box is the type you most commonly use to define deployments. It accepts commands in Bash, PowerShell, Salt, Ansible, Chef, or Puppet. ElasticBox provides Chef and Puppet public boxes to install and run recipes or manifests locally.

**In this article:**

* `Creating your first script box`_
* `Adding child script boxes`_

Creating Your First Script Box
------------------------------

On the Boxes page, click **New** > **Script**. Enter a name, optionally a description and other `metadata </../documentation/core-concepts/boxes/#box-metadata>`_. Save to continue. Configure the deployment using `events </../documentation/configuring-and-managing-boxes/start-stop-and-upgrade-boxes/>`_ and `variables </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/>`_.

When ready to test the configuration, click **Deploy**. Under the Deployment Box, search or select a deployment policy. Policies whose claims match the script box requirements appear here.

Don't find what you need? Then click **Create a new deployment policy box**.

The easiest way to understand script boxes is to build one. Follow this tutorial to build a simple box that says `Hello World </../documentation/getting-started/hello-world-in-elasticbox/>`_.

Adding Child Script Boxes
-------------------------

Let's build on top of the `Hello World </../documentation/getting-started/hello-world-in-elasticbox/>`_ box as an example. To set up full-scale application deployments, you need to stitch components or micro components together. You do that by stacking child boxes within a parent. In this example, we'll stack the Hello World box within another box.

Create a new Script box and call it Greeting. Tag that it needs Linux. To learn more, see requirements and auto updates under `Box Basics </../documentation/core-concepts/boxes/#anatomy>`_.

.. raw:: html

    <div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/boxes/box-new-2.png" alt="Create a New Box">
    </div>

Now add a new variable of type **Box**. This allows the greeting box to consume the services of another. Click **New** in the variable section and add a box variable as shown. Set the variable name to GREETER and for the value, select the **Hello World** box.

.. raw:: html

    <div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/boxes/box-new-box-variable.png" alt="Add Box Type Variable">
    </div>

Now that Hello World is nested in the Greeting box, we can replace Hello World box variables with values we want. To do this, expand the GREETER variable and edit (click the pencil icon) the GREETING sub variable.

.. raw:: html

    <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/boxes/box-box-variables.png" alt="Select the Box Variable Sub-Variable">
    </div>

Edit this to say hello to someone else.

.. raw:: html

	<div class="doc-image padding-1x">
     	<img class="img-responsive" src="/../assets/img/docs/boxes/box-edit-variable.png" alt="Edit the Value of the Nested Box Variable">
    </div>

Now we overwrote the original value of the GREETING variable from the Hello World box. To go back to its original value, we can click the trash can icon to the right of the pencil icon.

**Note**: You can quickly tell which variables values are overridden because they change from italicized to regular text.

.. raw:: html

	<div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/boxes/box-box-variables-2.png" alt="See Value of Sub-Variable Overridden">
    </div>

See what we did? We consumed a box configuration and changed deployment values of the child box within the context of the parent. Remember that the original child box definition of Hello World, in this case, is not affected. When you deploy the Greeting box in this example, it also deploys the Hello World box in the same instance.

