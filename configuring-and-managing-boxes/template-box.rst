Template Boxes
**************

* `Deploy Using CloudFormation Templates`_
* `Deploy Using Azure Resource Manager Templates`_

Deploy Using CloudFormation Templates
--------------------------------------

The ElasticBox CloudFormation box type runs on the `AWS CloudFormation service <http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html>`_.

ElasticBox works with the AWS CloudFormation API to provision the stack in your AWS account. So why use ElasticBox when you can launch CloudFormation templates directly in Amazon? `Here's why </../documentation/>`_.

ElasticBox supports all CloudFormation templates available from AWS. Leverage services such as EC2, Elastic Block Store, Simple Notification Service, Elastic Load Balancing and Auto Scaling, RDS, S3, DynamoDB, Elastic IPs, and much more.

`AWS RDS services </../documentation/deploying-and-managing-instances/using-your-aws-account/#aws-rds>`_ such as MySQL, MS SQL, PostgreSQL, Oracle, and `Memcached </../documentation/deploying-and-managing-instances/using-your-aws-account/#aws-memcached>`_, `S3 </../documentation/deploying-and-managing-instances/using-your-aws-account/#aws-s3>`_, `DynamoDB </../documentation/deploying-and-managing-instances/using-your-aws-account/#aws-dynamodb>`_ are readymade CloudFormation templates. To use these services, configure a CloudFormation box of the type and select an AWS account registered in ElasticBox.

In this article:

* `Create a CloudFormation Template and Launch a Stack`_
* `Update a CloudFormation stack in real-time`_
* `Connect to other CloudFormation boxes over bindings`_

Create a CloudFormation Template and Launch a Stack
---------------------------------------------------

The CloudFormation box consists mainly of a template where you describe all the AWS resources you need to run your application. ElasticBox parses the template and automatically shows input parameters under a section called Variables. This enables you to customize a template easily.

We use a sample Wordpress template to show how to create and launch a CloudFormation template in ElasticBox.

Step 1. Create the template
```````````````````````````

1. `Log in to ElasticBox <http://elasticbox.com/login>`_.

2. Click Boxes > New > Template > CloudFormation Template. Give the box a meaningful name to identify it in the box service catalog. Specify other `metadata </../../documentation/core-concepts/boxes/#box-metadata>`_.

    .. raw:: html

      <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-box-definebasicmetadata.png" alt="Select CloudFormation Box Type">
      </div>

3. In the box, select **New** in Template, under Code tab. In this walkthrough, we import a `sample WordPress template <https://s3.amazonaws.com/cloudformation-templates-us-east-1/WordPress_Single_Instance_With_RDS.template>`_ from a URL. When we save, contents from the URL are ported over.

    .. raw:: html

    	<div class="doc-image padding-1x">
    		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-box-createtemplate.png" alt="Create a CloudFormation Template">
    	</div>

    Besides URL, you have a couple of other options to create a template:

    * **Blank Template**. Develop one from scratch. When you save, you have a blank template you can start authoring.

    * **File**. Upload an existing template. When you save, the contents of the file are available in the template. You can upload one up to 1MB in size.

    **Note**: When you import from a file or a URL, make sure its content is formatted in JSON and follows the CloudFormation template conventions.

Step 2. Author the template
```````````````````````````

1. Start with a `sample AWS CloudFormation template <https://aws.amazon.com/cloudformation/aws-cloudformation-templates/>`_ and click the pencil to modify. Here we use the sample WordPress template.

    .. raw:: html

    	<div class="doc-image padding-1x">
    		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-authortemplate.png" alt="Author the Template">
    	</div>

    **Note**: CloudFormation templates have their own taxonomy you must follow. Although a template typically has several sections, only Resources is required. For information and examples on how to declare each section, see `Template Anatomy <http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html>`_ and `Template Reference <http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-reference.html>`_.

2. Customize input parameters. Although optional, if you have them in the template, they're automatically shown under Variables. You can customize several parameters as in this example.

    .. raw:: html

    	<div class="doc-image padding-1x">
    		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-modifyparameters-undervariables.png" alt="Modify Parameters Under Box Variables">
    	</div>

    You can customize further by adding parameters under Variables. In this example, we added an Options variable to set the database engine version at deploy time. When we save the variable, notice how it’s automatically added as a parameter to the template in the correct JSON format.

    .. raw:: html

    	<div class="doc-image padding-1x">
    		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-customize-templateparameters.png" alt="Add or Update Template Parameters">
    	</div>

    Variables in CloudFormation boxes:

    * The template accepts only String, Number, or CommaDelimitedList types. So any variables you add to the box are converted to one of these types. Text, file, password, URL, and email variables are treated as string parameters. Number and port variables are treated as number parameters.

    * Bindings have a special use and are explained later in this walkthrough.

    * Variables imported from a template are always required at deploy time even if you don't flag them as such in the box. Since they must contain values at launch time, you can set a default value when creating them or supply them at deploy time.

    * At this time, `box type variables </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-boxtype>`_ are not supported.

    * The file variable is a useful way to include a script that you want to execute in your stack. When you add a file, ElasticBox stores it on a secure server and declares the file variable as a parameter with a URL value in the parameters section of the template. To execute the file, you can add a script in the user data section of the template. Or depending on your resource type, reference it from the resource properties section. One example for using a file is to store it in the S3 bucket that you launch as part of the stack.

    **Note**: As you’re authoring, it’s important to check that the template is valid. While ElasticBox validates the correctness of the JSON format and the template syntax correctness, we can’t know whether resources specified are available in your AWS account or whether property values of a resource are valid. For that level of checking, it’s best to test launch the CloudFormation box instance from ElasticBox and refine the template in real-time.

Step 3. Launch the CloudFormation stack
```````````````````````````````````````

1. On the box page, click **Deploy**.

2. For Deployment Policy, select an AWS CloudFormation Deployment Policy added in ElasticBox to indicate the location and the availability zone to launch the stack.

    .. raw:: html

    	<div class="doc-image padding-1x">
    		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-launchstack-settings.png" alt="Select Deployment Settings">
    	</div>

3. Optionally, add tags for `bindings </../documentation/configuring-and-managing-boxes/template-box/#connect-to-other-cloudformation-boxes-over-bindings/>`_, `auto schedule the instance </../documentation/deploying-and-managing-instances/deploying-managing-instances/#instance-scheduler>`_, and set `auto updates </../documentation/core-concepts/boxes/#box-metadata>`_.

4. Under Variables, set values for each parameter based on the **AllowedValues** property in the template parameters section.

5. Click **Deploy** to launch the stack.

    **Note**: When launched successfully, website URL is available in the instance lifecycle editor. Click **Lifecycle Editor** on the instance page and look under WebsiteURL.

    .. raw:: html

    	<div class="doc-image padding-1x">
    		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-stack-outputs.png" alt="Access CloudFormation Stack Outputs in the Lifecycle Editor">
    	</div>

Update a CloudFormation Stack in Real-Time
------------------------------------------

Once live, you can continue to make changes to your CloudFormation template from the instance lifecycle editor and test in real-time. Follow these steps.

**Steps**

1. `Log in to ElasticBox <http://elasticbox.com/login>`_.

2. Click Instances and select the CloudFormation instance you want to update. In this example, we’ll select the WordPress instance launched earlier.

3. On the instance page, click **Lifecycle Editor**.

    .. raw:: html

    	<div class="doc-image padding-1x">
            <img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-stack-update.png" alt="Update the CloudFormation Stack in the Lifecycle Editor">
        </div>

4. Update the template and test launch the stack. You can change any section of the template or rewrite it entirely. When ready to update the stack in AWS, click **Reconfigure**. In this example, we increased the RDS database size by changing the value of the DBAllocatedStorage parameter.

    .. raw:: html

    	<div class="doc-image padding-1x">
            <img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-stack-updatewithreconfigure.png" alt="Update the CloudFormation Stack in Real-Time with Reconfigure">
        </div>

5. (Optional) Push updates back to the CloudFormation box. When you're satisfied changing and testing the template in the instance, you can push it back to the CloudFormation box as a version. To do this, click **New** under Versions tabs. This allows you or others in the future to choose a version that best suits your deployment.

     .. raw:: html

     	<div class="doc-image padding-1x">
    		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-savetemplatechanges-asnewversion.png" alt="Save Template Changes as a New Version to the Box">
    	</div>

Connect to Other CloudFormation Boxes over Bindings
---------------------------------------------------

Large CloudFormation deployments are challenging to manage in a single template. To simplify, break the template into smaller, manageable CloudFormation boxes and connect them with `bindings </../documentation/configuring-and-managing-boxes/managing-multi-tier-applications/>`_. Then use `text expressions </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-texttype>`_ to call the bindings. When you do, they're added to the parameter section of the template. At deploy time, the CloudFormation service calls the binding to connect and pass values between boxes.

To illustrate, we create a second CloudFormation box to scale the WordPress blog instance automatically when past its load limit. In the following steps, we add a binding and call it to connect the WordPress box to the autoscaling box.

**Steps**

1. Create a CloudFormation box using the `AWS autoscaling template <https://s3-us-west-2.amazonaws.com/cloudformation-templates-us-west-2/AutoScalingMultiAZWithNotifications.template>`_ and deploy it.

    .. raw:: html

    	<div class="doc-image padding-1x">
            <img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-bindings-createautoscalingbox.png" alt="Create a Autoscaling CloudFormation Box">
        </div>


2. Go to the WordPress box and add a binding to the Autoscaling box.

      .. raw:: html

      	<div class="doc-image padding-1x">
      		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-bindings-createbindingvar.png" alt="Create a binding in the CloudFormation Box">
      	</div>

3. When the WordPress box is deployed, the autoscalebinding variable must be matched with the Autoscaling Instance.

      .. raw:: html

      	<div class="doc-image padding-1x">
      		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-bindings-deploybindingvar.png" alt="Match with the Autoscaling Instance">
      	</div>

4. The relationship created by the binding is showed in the grid view.

      .. raw:: html

      	<div class="doc-image padding-1x">
      		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-bindings-gridview.png" alt="Grid view instances">
      	</div>

If some value of the binding is used in the WordPress box configuration, a text expression variable type must be created.

    Under Variables, click **New** and select the text expression variable type.

      .. raw:: html

      	<div class="doc-image padding-1x">
      		<img class="img-responsive" src="/../assets/img/docs/template-box/cloudformation-bindings-enterconnectionstring.png" alt="Enter Binding Connection String as a Text Expression">
      	</div>

    The expression can contain any string value or variables from templates. It can also contain system variables like instance, username, addresses. In general, follow this syntax: {{ binding_name.variable_name }}


Deploy Using Azure Resource Manager Templates
----------------------------------------------

The ElasticBox Azure Resource Manager Template box allows you to run any Azure service on ElasticBox. This allows you to use the power of ElasticBox (instance history, Lifecycle Editor, bindings, box versioning) combined with any service supported by Azure Resource Manager.

To learn more about:

How to write ARM Templates `see <https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates>`_.
The many services available in Azure take a look at the official documentation `here <https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-supported-services>`_

In this article:

* `Create an Azure Resource Manager Template and Launch a Stack`_
* `Update an Azure Resource Manager Stack in Real-Time`_
* `Connect to Other Azure Resource Manager Boxes over Bindings`_

Create an Azure Resource Manager Template and Launch a Stack
--------------------------------------------------------------

The Azure Resource Manager Template box consists mainly of a template where you describe all the AWS resources you need to run your application. ElasticBox parses the template and automatically shows input parameters under a section called Variables. This enables you to customize a template easily.

We use a sample Wordpress template to show how to create and launch a Azure Resource Manager template in ElasticBox.

Step 1. Create the template
````````````````````````````

1. `Log in to ElasticBox <http://elasticbox.com/login>`_.

2. Click Boxes > New > Template > Azure Resource Manager Template. Give the box a meaningful name to identify it in the box service catalog. Specify other `metadata </../../documentation/core-concepts/boxes/#box-metadata>`_.

    .. raw:: html

      <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/template-box/azr-box-definebasicmetadata.png" alt="Select Azure Resource Manager Box Type">
      </div>

3. In the box, select **New** in Template, under Code tab. In this walkthrough, we import a `sample WordPress template <https://s3.amazonaws.com/cloudformation-templates-us-east-1/WordPress_Single_Instance_With_RDS.template>`_ from a URL. When we save, contents from the URL are ported over.

    .. raw:: html

      <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/template-box/azr-box-createtemplate.png" alt="Create a Azure Resource Manager Template">
      </div>

    Besides URL, you have a couple of other options to create a template:

    * **Blank Template**. Develop one from scratch. When you save, you have a blank template you can start authoring.

    * **File**. Upload an existing template. When you save, the contents of the file are available in the template. You can upload one up to 1MB in size.

    **Note**: When you import from a file or a URL, make sure its content is formatted in JSON and follows the Azure Resource Manager template conventions.

Step 2. Author the template
```````````````````````````

1. Start with a `sample Azure Resource Manager template <https://github.com/Azure/azure-quickstart-templates/>`_ and click the pencil to modify.

    .. raw:: html

      <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/template-box/azr-authortemplate.png" alt="Author the Template">
      </div>

    **Note**: For more information on creating templates, please refer to the official `documentation <https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates>`_.

2. Customize parameters. Although optional, if you have them in the template, they're automatically shown under Variables. You can customize several parameters as in this example.

    .. raw:: html

      <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/template-box/azr-modifyparameters-undervariables.png" alt="Modify Parameters Under Box Variables">
      </div>

    Variables in Azure Resource Manager boxes:

    * Bindings have a special use and are explained later in this walkthrough.

    * Variables imported from a template are always required at deploy time even if you don't flag them as such in the box. Since they must contain values at launch time, you can set a default value when creating them or supply them at deploy time.

    * The text variable can be parametrized through Jinja, for example to use binding information. See more documentation about his `here </../documentation/configuring-and-managing-boxes/syntax-for-variables/>`_ 

    * At this time, `box type variables </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-boxtype>`_ are not supported.

    * The file variable is a useful way to include a script that you want to execute in your stack. When you add a file, ElasticBox stores it on a secure server and declares the file variable as a parameter with a URL value in the parameters section of the template. To execute the file, you can add a script in the user data section of the template. Or depending on your resource type, reference it from the resource properties section. One example for using a file is to store it in the S3 bucket that you launch as part of the stack.

    **Note**: As you’re authoring, it’s important to check that the template is valid. While ElasticBox validates the correctness of the JSON format and the template syntax correctness, we can’t know whether resources specified are available in your AWS account or whether property values of a resource are valid. For that level of checking, it’s best to test launch the Azure Resource Manager box instance from ElasticBox and refine the template in real-time.

Step 3. Launch the Azure Resource Manager stack
````````````````````````````````````````````````

1. On the box page, click **Deploy**.

2. For Deployment Policy, select an ARM Deployment Policy Box added in ElasticBox to indicate the location and the availability zone to launch the stack.

    .. raw:: html

      <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/template-box/azr-launchstack-settings.png" alt="Select Deployment Settings">
      </div>

3. Optionally, add tags for `bindings </../documentation/configuring-and-managing-boxes/template-box/#connect-to-other-cloudformation-boxes-over-bindings/>`_, `auto schedule the instance </../documentation/deploying-and-managing-instances/deploying-managing-instances/#instance-scheduler>`_, and set `auto updates </../documentation/core-concepts/boxes/#box-metadata>`_.

4. Under Variables, set values for each parameter based on the **AllowedValues** property in the template parameters section.

5. Click **Deploy** to launch the stack.

    **Note**: When launched successfully, website URL is available in the instance lifecycle editor. Click **Lifecycle Editor** on the instance page and look under WebsiteURL.

    .. raw:: html

      <div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/template-box/azr-stack-outputs.png" alt="Access Azure Resource Manager Stack Outputs in the Lifecycle Editor">
      </div>

Update an Azure Resource Manager Stack in Real-Time
---------------------------------------------------

In the LCE, you can update the template and variables to change your current deployment.

ElasticBox will check the different resources and update the ones that need it in your instance Resource Group to match your new template.

Please, check with Azure documentation to know which live updates are allowed and which resources will be destroyed and redeployed.

Connect to Other Azure Resource Manager Boxes over Bindings
------------------------------------------------------------

Large Azure Resource Manager deployments are challenging to manage in a single template. To simplify, break the template into smaller, manageable ARM boxes and connect them with `bindings </../documentation/configuring-and-managing-boxes/managing-multi-tier-applications/>`_. Then use `text expressions </../documentation/configuring-and-managing-boxes/parameterizing-boxes-with-variables/#box-creating-texttype>`_ to call the bindings. When you do, they're added to the parameter section of the template. At deploy time, the Azure Resource Manager service calls the binding to connect and pass values between boxes.

To illustrate, we create a second Azure Resource Manager box to scale the WordPress blog instance automatically when past its load limit. In the following steps, we add a binding and call it to connect the WordPress box to the autoscaling box.

**Steps**

1. Go to an Azure Resource Manager box.

2. Add a binding to the Azure Resource Manager box.

      .. raw:: html

        <div class="doc-image padding-1x">
          <img class="img-responsive" src="/../assets/img/docs/template-box/azr-bindings-createbindingvar.png" alt="Create a binding in the Azure Resource Manager Box">
        </div>
