Application Boxes
********************************

The Application box deploys several instances that execute an application. An application box define a topology of `Script boxes </../documentation/configuring-and-managing-boxes/script-box/>`_, `CloudFormation boxes </../documentation/configuring-and-managing-boxes/cloudformation-box/>`_ and `Container boxes </../documentation/configuring-and-managing-boxes/docker-container-service/>`_

**In this article**:

* `What are application boxes?`_
* `Creating application boxes`_

What are application boxes?
-------------------------------

To deploy most applications, you need several instances cooperating together. Application boxes are a way to define and reuse how several boxes will work together to run an application.

What differences Application Boxes from Script Boxes is that deploying one application box creates several instances, while deploying a Script Box creates one instance. You can think of an application box as a way to simplify deploying several instances.

An application box defines a topology. A topology is a list of boxes and all the information needed to deploy them. This way you can launch several instances to work together.

.. raw:: html

	<div class="doc-image padding-1x">
        <div class="browser-feature">
          <div class="indicators">
              <div class="circle magenta"></div>
              <div class="circle orange"></div>
              <div class="circle green"></div>
            </div>
            <div class="browser-window">
              <img class="img-responsive" src="/../assets/img/docs/boxes/application-box-multitier-example.png" alt="A multitier application box showing all the boxes in the topology">
            </div>
        </div>
      </div>

This makes deploying several instances fast and prevent errors. The instances deployed can be managed separately if needed, allowing you to adapt each running application as needed. Each box in the topology includes the information needed to deploy it as an instance.

Creating application boxes
-------------------------------

To create an application box just go to the boxes page and click new and select application box. You only need to configure the name of the application box.

The application box contains a topology. A topology is a description of the required boxes and how they will be deployed as instances. For each box you can define:

* The name of the resulting instance
* The version of the box
* The tags that the instance will have
* The requirements needed to be fulfilled by its policy box
* The value of each variable of the box

.. raw:: html

	<div class="doc-image padding-1x">
        <img class="img-responsive" src="/../assets/img/docs/boxes/application-box-add-box.png" alt="A multitier application box showing all the boxes in the topology">
    </div>

This is equivalent to the values you need to provide to deploy that box as an instance, except the policy box and requirements. When you deploy other types of boxes, you must select one policy box for an instance. The box includes a list of requirements that must appear as claims in the Policy box.

In Application boxes, a policy box is automatically selected for each box in the topology. If you need different boxes of the topology to have different policies, you can simply add restrictions in the Requirements field.

For example, if you want to deploy an application across regions, you could add a requirement zone-a for some of the boxes and zone-b for others. That way different instances will be run with different policies, going to different regions.

After adding all the boxes to the topology, you can deploy the application box clicking on deploy and all the configuration will be deployed for you.
