Setting Up CI/CD with ElasticBox, Jenkins and Bitbucket
*************************************************************

The ElasticBox Jenkins plugin automates CI/CD on any cloud and SCM. In this article, we use Bitbucket as the SCM.

**Note**: To get started, you need a Jenkins server with Bitbucket and ElasticBox plugins.

To add ElasticBox build steps in Jenkins jobs, go to the job page. Under Build, click **Add Build Step** and select an ElasticBox deploy, manage, or update step.

**In this article:**

* `Previous requirements`_
* `Setting up the continuous integration environment`_
* `Reacting to changes in Bitbucket`_
* `Pull Request lifecycle`_

Previous requirements
-----------------------

1. You will need a Bitbucket repository.
	Example: In this `repo <https://bitbucket.org/oserna/hello-world-war>`_ you can find a Java web application:

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/bitbucket-sample-repo-overview.png" alt="Bitbucket sample repo overview">
		</div>

	The repo was cloned in the local environment:

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/sourcetree-local-repo-overview.png" alt="Sourcetree local repo overview">
		</div>

2. You will need a Jenkins instance up and running.
	In our case we are going to deploy the latest stable Jenkins version using a box created in ElasticBox. We created this box for future use when we need to provision a Jenkins environment again. At this point, all we need to do is to deploy this box and a Jenkins master with all the plugin configurations will be setup within minutes.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/elasticbox-jenkins-box-overview.png" alt="ElasticBox Jenkins box overview">
		</div>

3. We will need an Apache Tomcat 7.
	In order to keep it simple we decided to create a tomcat box in ElasticBox to deploy the apache tomcat server.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/elasticbox-tomcat-box-overview.png" alt="ElasticBox Tomcat box overview">
		</div>

Setting up the continuous integration environment
---------------------------------------------------

The objective is to demonstrate how easy it is to set up a continuous integration environment using Jenkins, the ElasticBox Jenkins plugin and Bitbucket as repository.

Below is a diagram of workflow and how all the components work together.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/bitbucket-jenkins-integration-overview.png" alt="Bitbucket and Jenkins integration overview">
	</div>

1. Set up the Jenkins instance (in our case we will deploy the box we have created previously).

	1. Deploy the box.

		.. raw:: html

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/deployment-option.png" alt="Deployment option">
			</div>

	2. Choose the policy box.

		.. raw:: html

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/jenkins-box-ui.png" alt="Jenkins box in ElasticBox UI">
			</div>

	3. Deployed

		.. raw:: html

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/elasticbox-jenkins-box-deployed.png" alt="ElasticBox Jenkins box deployed">
			</div>

2. Configure the ElasticBox cloud in the Jenkins Manager section.

	.. raw:: html

		<div class="doc-image padding-1x">
		  <div class="browser-feature">
		    <div class="indicators">
		        <div class="circle magenta"></div>
		        <div class="circle orange"></div>
		        <div class="circle green"></div>
		      </div>
		      <div class="browser-window">
		        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/jenkins-configuration-page.png" alt="Jenkins configuration page">
		      </div>
		  </div>
		</div>

	In the cloud section we add the ElasticBox cloud:

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/elasticbox-cloud-configuration-in-jenkins.png" alt="ElasticBox cloud configuration in Jenkins">
		</div>

3. Create a Jenkins job in order to see all the pieces working together.

	.. raw:: html

		<div class="doc-image padding-1x">
		  <div class="browser-feature">
		    <div class="indicators">
		        <div class="circle magenta"></div>
		        <div class="circle orange"></div>
		        <div class="circle green"></div>
		      </div>
		      <div class="browser-window">
		        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/new-jenkins-job-just-created.png" alt="New Jenkins job just created">
		      </div>
		  </div>
		</div>

4. Now we are going to configure the job:

	.. raw:: html

		<div class="doc-image padding-1x">
		  <div class="browser-feature">
		    <div class="indicators">
		        <div class="circle magenta"></div>
		        <div class="circle orange"></div>
		        <div class="circle green"></div>
		      </div>
		      <div class="browser-window">
		        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/configuration-option.png" alt="Configuration option">
		      </div>
		  </div>
		</div>

5. In the Source Code Management section we have to configure the Bitbucket repository (Git should be installed in the Jenkins node):

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/source-code-management-configuration.png" alt="Source Code Management configuration">
		</div>

	In this case the url will be: **https://oserna@bitbucket.org/oserna/hello-world-war.git**

	Enter your credentials in order to access the repository.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/bitbucket-repo-access-credentials.png" alt="Bitbucket repository access credentials">
		</div>

6. In the Build section we add a DeployBox build step in order to deploy our previously created Tomcat box.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/deploybox-stepbuilder-configuration.png" alt="DeployBox stepbuilder configuration">
		</div>

	In the previous schema you saw that we selected the Tomcat box with its latest version. We also added the tag “tomcat” to that instance in order to easily locate them in our cloud. In this case, we have selected to deploy this box in the Google Compute Engine (GCE) using the policy box below:

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/elasticbox-policy-box-overview.png" alt="ElasticBox policy box overview">
		</div>

7. Now, once we have configured the Tomcat box that will be deployed, we need to build the source code that we have downloaded previously from the Bitbucket repository. Using a free style Jenkins job and not a maven Jenkins job, we will have to create a shell script step builder:

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/shell-script-step-builder-overview.png" alt="Shell script step-builder overview">
		</div>

	Apache Maven should be previously installed and added to the path in order to execute the order:

	.. raw:: html

		<pre>
		mvn clean install
		</pre>

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/maven-clean-install-command.png" alt="Maven clean install command">
		</div>

	As you might have noticed, the last command in the shell deploys the just war created package in Tomcat:

	.. raw:: html

		<pre>
		curl -v -T ${artifact} 'http://manager:manager@'${tomcat_host}':'${tomcat_port}'/manager/text/deploy?path=/'${context_path}''
		</pre>

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/curl-command-to-deploy.png" alt="Curl command to deploy the war">
		</div>

8. Let’s see how it works:

	1. The job gets the code from the repo:

		.. raw:: html

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/console-traces-getting-code-from-repo.png" alt="Traces in console getting the code from repo">
			</div>

	2. In the next step the job deploys the tomcat box.

		.. raw:: html

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/deploying-tomcat-box-traces.png" alt="Deploying tomcat box traces">
			</div>

	3. Now the job executes the build.

		.. raw:: html

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/build-console-traces.png" alt="Build console traces">
			</div>

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/testing-console-traces.png" alt="Testing traces in console">
			</div>

	4. Notice that it finally deploys the war package in the Tomcat server.

		.. raw:: html

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/deploying-war-package-traces.png" alt="Deploying war package traces">
			</div>

	5. And it works!!

		.. raw:: html

			<div class="doc-image padding-1x">
				<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/hello-world-from-tomcat.png" alt="Hello world page served from Tomcat">
			</div>

9. All the pieces working together.

	.. raw:: html

		<div class="doc-image padding-1x">
		  <div class="browser-feature">
		    <div class="indicators">
		        <div class="circle magenta"></div>
		        <div class="circle orange"></div>
		        <div class="circle green"></div>
		      </div>
		      <div class="browser-window">
		        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/all-console-traces-overview.png" alt="All console traces overview">
		      </div>
		  </div>
		</div>

10. Tomcat deployed with ElasticBox Jenkins plugin.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/already-deployed-instances.png" alt="Already deployed instances">
		</div>

Reacting to changes in Bitbucket
------------------------------------

In our previous case we spent some time setting up our continuous integration environment. We’ve started using Jenkins, Bitbucket and the ElasticBox Jenkins plugin, and so far we’re pretty happy. The next goal for us is to set up a Bitbucket service hook to trigger our builds.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/pushes-process-view.png" alt="Reacting to pushes process view">
	</div>

In the job that we created while setting up our continuous integration environment, we are going to enable notifications when a change is made in the Bitbucket repository. We do that in the Build trigger section in the configure job page.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/buid-trigger-configuration.png" alt="Buid trigger configuration to be aware of pushes">
	</div>

We now have to make the proper changes in order to enable the hooks from the Bitbucket repository. Adding webhooks pointing to our Jenkins CI server.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/bitbucket-webhooks-configuration-view.png" alt="Bitbucket webhooks configuration view">
	</div>

We make a change in our source code (previously cloned from the repository) within a local environment by a new <p> tag.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/changes-in-code.png" alt="Making a change in code">
	</div>

Once the change (the feature) is done we make the commit in our local repository and the push is made into the remote Bitbucket repository.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/changes-as-working-copy.png" alt="See the changes as working copy">
	</div>

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/commit-made-push-needed.png" alt="Once the commit has been made, the push is needed">
	</div>

After the push into the Bitbucket repository, the job is triggered.

.. raw:: html

	<div class="doc-image padding-1x">
	  <div class="browser-feature">
	    <div class="indicators">
	        <div class="circle magenta"></div>
	        <div class="circle orange"></div>
	        <div class="circle green"></div>
	      </div>
	      <div class="browser-window">
	        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/build-triggered.png" alt="See how the build is triggered">
	      </div>
	  </div>
	</div>

We will confirm that the result is what we expected, the package was properly created and deployed into the Tomcat server.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/deploy-war-traces.png" alt="Deploying war traces">
	</div>

We can see that our change is ready to be tested. And yes!!, our new paragraph is there.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/hello-world-tomcat.png" alt="See the hello world page served from Tomcat">
	</div>

Just being aware of the push changes in your repo provides many management possibilities depending on the type of development workflow in your company. You have to decide what push hooks will trigger Jenkins jobs. Such scenarios as every push event being able to trigger a Jenkins job or pushing events specific activities in a specific branch truly depends on your development workflow, and can be customized based on your business . As you can imagine the push event is the simplest and yet, most detailed way that you can handle an event. For example take the following classic Git workflow below:

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/git-workflow-branches.png" alt="Git workflow branches overview">
	</div>

So, assuming that you are using a branch that serves as an integration branch for features (this is the purple branch in the above schema), it may be enough (from the Jenkins perspective) to be notified about the pushes in the integration branch. A Jenkins job would be triggered every time that a new commit is added to the integration branch. The job could also send an email to whoever you want to be notified of build result or other actions that the job is able to do. However, better integration models would be more effective if we could be made aware (in Jenkins) about the different phases of the Pull Request lifecycle. Lets now cover this process next.

Pull Request lifecycle
------------------------

As you probably know, pull requests are a tool for developers to notify the rest of the team when a new feature is completed. This makes everyone aware that they need to revise the code before merging it from the feature branch into the master. So, being aware of every commit in the repo, as we did in the previous chapter, is cool, but if we’re able to build pull requests from Bitbucket and report the test results, that would be even better. Below you can see the Pull Request lifecycle as a part of our vision about how CI & CD can be implemented.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/pull-request-lifecycle-overview.png" alt="Pull request lifecycle overview">
	</div>

In the image below you can see the simplest implementation of the previous cycle which will be our working example that we’ll walk through in this setup.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/pull-request-lifecycle-implementation.png" alt="Simplest pull request lifecycle implementation">
	</div>

There are several ways to achieve this type of integration, depending on the mechanism involved, whether it be polling or pushing, and the type of repository you are using, Stash or Bitbucket Server.

Feel free to review the wiki pages for the simplest approach for using these Jenkins plugins together:

* `Git Jenkins plugin <https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin>`_
* `Bitbucket pull request builder Jenkins plugin <https://wiki.jenkins-ci.org/display/JENKINS/Bitbucket+pullrequest+builder+plugin>`_

In this case, our Jenkins server will poll the Bitbucket repository according to the time interval that we have chosen. We have created a Jenkins job to demonstrate how this method works which will configure the SCM as seen in the section below:

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/scm-git-jenkins-plugin-configuration-view.png" alt="SCM Git Jenkins plugin configuration view">
	</div>

We also need to configure the part related to the Bitbucket Pull Request Builder in the “Build Triggers” section:

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/pull-request-builder-plugin-configuration.png" alt="Pull request builder plugin configuration">
	</div>

Once we have configured our plugins we will see the result from the Bitbucket perspective. So, every time we make a new feature in a new branch, we have created the “master_branch_feature_3” containing commit 13:

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/new-branch.png" alt="A new branch for this feature">
	</div>

when we create the Pull Request in Bitbucket this is what we will see:

.. raw:: html

	<div class="doc-image padding-1x">
	  <div class="browser-feature">
	    <div class="indicators">
	        <div class="circle magenta"></div>
	        <div class="circle orange"></div>
	        <div class="circle green"></div>
	      </div>
	      <div class="browser-window">
	        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/no-build-passed.png" alt="See that there is no build passed">
	      </div>
	  </div>
	</div>

the commit inside:

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/commit-in-the-pull-request.png" alt="The commit in the pull request">
	</div>

Notice that we haven’t built the Pull Request in Jenkins yet (red figure in the first schema). But give it some time depending of the cron interval you set in the Jenkins job configuration and you will see the build being triggered and executed (see below):

.. raw:: html

	<div class="doc-image padding-1x">
	  <div class="browser-feature">
	    <div class="indicators">
	        <div class="circle magenta"></div>
	        <div class="circle orange"></div>
	        <div class="circle green"></div>
	      </div>
	      <div class="browser-window">
	        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/jenkins-execution-finished-succesfully.png" alt="Jenkins execution finished succesfully">
	      </div>
	  </div>
	</div>

Notice that the build was triggered because of commit 13.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/jenkins-job-triggered-by-the-pr-commit.png" alt="Jenkins job triggered by the PR commit">
	</div>

The branch being checked out is the master_branch_feature_3.

.. raw:: html

	<div class="doc-image padding-1x">
	  <div class="browser-feature">
	    <div class="indicators">
	        <div class="circle magenta"></div>
	        <div class="circle orange"></div>
	        <div class="circle green"></div>
	      </div>
	      <div class="browser-window">
	        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/traces-getting-repo-code.png" alt="Traces getting the repo code">
	      </div>
	  </div>
	</div>

And as soon as the build ends you will see the result in Bitbucket.

.. raw:: html

	<div class="doc-image padding-1x">
	  <div class="browser-feature">
	    <div class="indicators">
	        <div class="circle magenta"></div>
	        <div class="circle orange"></div>
	        <div class="circle green"></div>
	      </div>
	      <div class="browser-window">
	        <img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/build-passed-ok.png" alt="Notice that the build has passed ok">
	      </div>
	  </div>
	</div>

The build result notification:

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/bitbucket-build-ok.png" alt="Bitbucket shows that build was ok">
	</div>

By means of simplicity we decided to use the plugins combination that you saw above, but there are some other ways to integrate Bitbucket and Jenkins.

* `Post services in Bitbucket <https://confluence.atlassian.com/bitbucket/post-service-management-223216518.html>`_ (push based).

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/post-service-deprecated-message.png" alt="Post service deprecated message">
		</div>

	As seen above, **POST service management is deprecated** in favor of Webhooks 2.0. Please note that the Bitbucket Jenkins plugin only works for Bitbucket push events not for pull request events.

* `Jenkins CI broker service <https://confluence.atlassian.com/bitbucket/jenkins-service-management-251724180.html>`_ (push based too).

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/integrate-with-jenkins/jenkins-not-supported-service-integration-message.png" alt="Not supported Jenkins service integration message">
		</div>

	Currently, Atlassian Support does not provide assistance for this configuration. In addition, as you will notice, they redirect to `Jenkins Bitbucket Plugin <https://wiki.jenkins-ci.org/display/JENKINS/BitBucket+Plugin>`_, but that plugin only works for Bitbucket push events, not for pull request events.

* With other combination of plugins you could manage the pull request lifecycle too. For example using these plugins (having Stash but not Bitbucket):

	1. `Git Jenkins plugin <https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin>`_
	2. `Pre SCM Buildstep Jenkins plugin <https://wiki.jenkins-ci.org/display/JENKINS/pre-scm-buildstep>`_
	3. `Stash Notifier Jenkins plugin <https://wiki.jenkins-ci.org/display/JENKINS/StashNotifier+Plugin>`_
	4. `Pull Request Notifier Stash plugin <https://marketplace.atlassian.com/plugins/se.bjurr.prnfs.pull-request-notifier-for-stash/server/overview>`_

You can see all details `here <https://christiangalsterer.wordpress.com/2015/04/23/continuous-integration-for-pull-requests-with-jenkins-and-stash/>`_.