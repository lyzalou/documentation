Jenkins CI/CD Tutorial
**************************************

This tutorial is for you if you use Jenkins to automate software test and deployment workflows. It demonstrates how to use the `ElasticBox Jenkins Plugin <https://wiki.jenkins-ci.org/display/JENKINS/ElasticBox+CI>`_ to automate an entire end-to-end deployment. Here we use the plugin to automatically trigger Jenkins based on a code checkin in GitHub to deploy a LAMP Stack app in a test environment.

The LAMP Stack app is defined in ElasticBox as a box that you can predictably deploy not just in testing, but also in staging and production environments the same way.

It takes 90 minutes to go through this tutorial. By the end, you'd know how to use the plugin to integrate with your own Jenkins setup and automate deployments painlessly.

**In this article:**

* `Before you begin`_
* `Create a Jenkins build job`_
* `Trigger a CI/CD build`_

Before You Begin
-------------------

You need the following:

* Get an ElasticBox account. You can `sign up <https://elasticbox.com/#>`_ for free.
* Fork a repository of this `GitHub project <https://github.com/ElasticBox/Easy-PHP-MySQL>`_. You'll use this to make some code changes and trigger a Jenkins build.
* Build a `LAMP Stack box with a binding to a MySQL Database box </../documentation/sample-tutorials/lamp-stack/#lamp-defineappwithbinding>`_. Create and save a `deployment profile </../documentation/sample-tutorials/lamp-stack/#lamp-deployapp>`_ for both boxes, but don't deploy.
* `Install Jenkins server </../documentation/integrate-with-jenkins/jenkins-elasticbox-setup/#jenkins-create-deploy-box>`_ and `set it up </../documentation/integrate-with-jenkins/jenkins-elasticbox-setup/#jenkins-configure-plugin>`_.
* Define a `slave box configuration </../documentation/integrate-with-jenkins/jenkins-elasticbox-slaves/#jenkins-configure-slave-box>`_ in ElasticBox and `configure Jenkins to build using the slave </../documentation/integrate-with-jenkins/jenkins-elasticbox-slaves/#jenkins-configure-plugin>`_.

Create a Jenkins Build Job
-----------------------------

Now that you've installed Jenkins server and registered your ElasticBox account there, you’re ready to create a job that automates a test build.

**Step 1. Create a new job based on an existing template**

When you install Jenkins server you get a pull-request job template. Create a new project called test-pullrequest and copy the template.

.. raw:: html

	<div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial4.png" alt="Create a New Jenkins Job">
          </div>
      </div>
    </div>

**Step 2. Select a slave from ElasticBox to run the job**

In the build job, select **Restrict where this project can be run**. Type the label for the slave box you added when `setting up Jenkins server </../documentation/integrate-with-jenkins/setting-up-ci-cd/#jenkins-configure-plugin>`_.

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial5.png" alt="Select Jenkins Slave to Run the Job">
	</div>

**Step 3. Automate pull request triggers**

Since we're using GitHub to submit code changes via pull requests, we'll manage the lifecycle of pull requests through the ElasticBox Jenkins plugin. Under Build Triggers, check **ElasticBox GitHub Pull Request Lifecycle Management**.

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial-git-pullrequestmgr.png" alt="Check the Option to Manage GitHub Pull Request Lifecycle">
	</div>

This setting triggers the job to build when you create a new pull request or update it. When you close the pull request, it triggers the job to delete the instances associated with the pull request saving compute resources in the process.

**Step 4 (optional). Add Shell commands to run unit tests**

Under Build > Add build step, click **Execute shell**. Enter Bash Shell commands to run any unit tests on your pull request.

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial8.png" alt="Add an Execute Shell Build Step">
	</div>

**Step 5. Deploy the MySQL Database Service box**

Under Build > Add build step, click **ElasticBox - Deploy Box**. Select the MySQL Database Service box from your workspace. To select the `deployment profile </../documentation/deploying-and-managing-instances/deploying-managing-instances/#profile>`_ here, you have to create it for the box in your workspace first.

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial9.png" alt="Add a Build Step to Deploy the MySQL Database Service">
	</div>

* Make sure you select **ElasticBox Automatic Version** as this is a default box.
* Tag the instance with a unique label to indetify it in the pull request.
* Set the instance to terminate 6 hours from when it launches.
* Under **Variables**, enter a username, password for the database. Without these values, the plugin can't deploy the MySQL Database Service.
* Under Advanced, select **skip deployment** for **If instance already exists**. This avoids deploying a new MySQL Database Service if you’re only changing an existing pull request.

**Step 6. Deploy the LAMP Stack box**

This step deploys the LAMP Stack application box and binds to the MySQL Database Service instance defined in the step above. This is how you deploy multi-tier applications from a single job.

Under Build > Add build step, click **ElasticBox - Deploy Box**. As before, select your ElasticBox account registered in Jenkins, your workspace where the LAMP Stack box is located, the LAMP Stack box, a version, and a deployment profile to launch it.

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial10.png" alt="Add a Build Step to Deploy the LAMP Stack App">
	</div>

* Schedule a time to terminate the instance under Expiration.
* Under Variables, bind to the mysql_service box by providing its tag as shown.
* Click **Advanced**. Under If instance already exists, choose reconfigure. This updates the LAMP Stack instance with the latest pull request changes without deploying again.

**Step 7 (optional). Add Shell commands to test the app**

This step optionally runs any shell commands you give to test the LAMP Stack application.

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial11.png" alt="Add Shell Commands">
	</div>

**Step 8. Enter your own GitHub project URL in the job**

Enter your own forked URL of the `GitHub project <https://github.com/ElasticBox/Easy-PHP-MySQL>`_ in these fields: **GitHub project, Git Repositories**, and **CLONE_URL**.

Here's an example:

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial12.png" alt="Enter Your Own Forked URL of the GitHub Project">
	</div>

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial12-1.png" alt="Enter Your Own Forked URL of the GitHub Project">
	</div>

**Step 9. Enable build, save job, and verify**

You’re almost done. Remember we copied this job from the template that's disabled? To enable, deselect **Disable Build**.

.. raw:: html

	<div class="doc-image padding-1x">
    	<img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial13.png" alt="Enable Build Job">
	</div>

Save the job. Verify that it's configured properly by checking the logs. Under Manage Jenkins > System Log, Jenkins should show something like this:

.. raw:: html

	<pre>
	Feb 04, 2015 1:58:03 AM INFO com.elasticbox.jenkins.triggers.github.PullRequestBuildHandler 
	Adding webhook http://107.178.218.51:8080/elasticbox/ to GitHub repository https://github.com/Mrinan/Easy-PHP-MySQL/
	Feb 04, 2015 1:58:03 AM INFO com.elasticbox.jenkins.triggers.github.PullRequestBuildHandler createWebHook
	Webhook http://107.178.218.51:8080/elasticbox/ is added to GitHub repository https://github.com/Mrinan/Easy-PHP-MySQL/
	</pre>

Trigger a CI/CD Build
------------------------

The test-pullrequest job is now all set to build, test, and deploy any pull request you submit to your forked `GitHub project <https://github.com/ElasticBox/Easy-PHP-MySQL>`_.

**Step 1. Submit a pull request**

To do this, install GitHub for your desktop. Create a new branch based on your forked repo master and commit a change to index.php in this line:

.. raw:: html

	<pre>
    print "&#60;HTML&#62;&#60;BODY&#62;&#60;H1&#62;Showing the users of the sampledb that has been created in the RDS some seconds ago!&#60;/H1&#62;\n";
	</pre>

Make sure you compare the pull request against your own master branch as seen here.

.. raw:: html

	<div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial22.png" alt="Compare Pull Request Against Your Own Master Branch">
          </div>
      </div>
    </div>

Go ahead, submit a pull request from your forked repo as shown.

.. raw:: html

	<div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial15.png" alt="Submit a GitHub Pull Request">
          </div>
      </div>
    </div>

Soon as you create the request, you should see a build trigger automatically.

.. raw:: html

	<div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial16.png" alt="See Build Triggered Automatically">
          </div>
      </div>
    </div>

**Step 2. Check your workspace for deployed instances**

Do you see the slave, LAMP Stack, and database instances online?

.. raw:: html

	<div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial17.png" alt="See Instances Online">
          </div>
      </div>
    </div>

Click the LAMP Stack instance to verify that your pull request changes are there.

.. raw:: html

    <div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial18.png" alt="Select LAMP Stack App Website IP Address">
          </div>
      </div>
    </div>

    <div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial19.png" alt="See Pull Request Changes">
          </div>
      </div>
    </div>

**Step 3. Check Jenkins build log**

In Jenkins, look for the build job log at **http://<your Jenkins host>/job/test-pullrequest/lastBuild/console**. See that the build ran successfully for this specific job.

.. raw:: html

    <div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial20.png" alt="See Build Ran Successfully in the Logs">
          </div>
      </div>
    </div>

**Step 4. You're ready to merge**

When the build finishes, a message is automatically posted to your GitHub request. Since you've verified changes in the test instance, you can now merge them through another build job in Jenkins.

.. raw:: html

    <div class="doc-image padding-1x">
      <div class="browser-feature">
        <div class="indicators">
            <div class="circle magenta"></div>
            <div class="circle orange"></div>
            <div class="circle green"></div>
          </div>
          <div class="browser-window">
            <img class="img-responsive" src="/../assets/img/docs/tutorials/Jenkins-tutorial21.png" alt="Check that You Got the Successful Build Message in the Pull Request">
          </div>
      </div>
    </div>

See how easy that was to automate a test build for code you checked in? If you want to take it a step further, set up a merge job using the merge job template to automatically deploy changes to staging, and then to production.
