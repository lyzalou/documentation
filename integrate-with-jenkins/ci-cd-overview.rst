Overview of the Jenkins ElasticBox Plugin
*******************************************

If you use Jenkins to continuously test and integrate code changes in development, staging, or production, you'd want to use the `Jenkins ElasticBox plugin <https://wiki.jenkins-ci.org/display/JENKINS/ElasticBox+CI>`_ to fully automate touchless deployments.

Here is a typical continuous integration and deployment (CI/CD) workflow that starts with developers checking in code and goes a full circle to launch changes in development, testing, staging, and production. See how ElasticBox automates the workflow in every step of the way.

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/jenkins-scm-ebplugin-architecture-workflow.png" alt="Jenkins ElasticBox Architecture and Workflow">
    </div>

**Supported CI/CD scenarios**

* **Deploy with zero clicks involved**. After you set up Jenkins with source control management (SCM) plugins, the ElasticBox plugin, and build jobs, code changes automatically trigger Jenkins to test in development, or merge them into staging and production environments. The plugin deploys to your apps from boxes to any cloud or infrastructure.
* **Deploy complex multi-tier apps and manage their lifecycle**. Launch multi-tier app stacks from ElasticBox through a single Jenkins build job using flexible binding references. Trigger lifecycle actions on the apps automatically by adding them to build jobs.
* **Orchestrate large-scale deployments without downtime**. Automate deployments at scale with autoscaling and load balancing in AWS or Azure. Make sure your apps are always available and protected from failures, data loss, and downtime as you continuously integrate and deploy in production.
* **Launch slaves on demand**. Configure slaves to automatically execute build jobs concurrently. Slaves help to speed up builds, build in a specific OS, runtime stack, or build changesets for small or large dev teams.

Without the plugin, normally you'd have to manually set up slaves, provision each environment like testing, staging, production with a cloud provider or your own infrastructure, and then you have to configure applications to deploy in those environments. Besides laborious manual setup, you'd have to make sure applications deploy consistently. You can solve all these challenges with the ElasticBox Jenkins plugin.

**Next Up**

* `Setting Up Jenkins with ElasticBox </../documentation/integrate-with-jenkins/jenkins-elasticbox-setup/>`_
* `Launching Slaves via ElasticBox </../documentation/integrate-with-jenkins/jenkins-elasticbox-slaves/>`_
* `Setting Up CI/CD with ElasticBox and Jenkins </../documentation/integrate-with-jenkins/setting-up-ci-cd/>`_
* `Jenkins CI/CD Tutorial </../documentation/sample-tutorials/jenkins-elasticboxplugin/>`_

