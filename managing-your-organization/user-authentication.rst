Enable User Authentication
********************************

In ElasticBox enterprise organizations, users can sign in using any of the single sign-on authentication options you enable in the `admin console </../documentation/managing-your-organization/admin-overview/>`_.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/adminconsole/admin-console-enable-user-authenticaion.png" alt="Authentication and Single Sign-On">
	</div>

**In this article:**

* `Single sign-on with Google, GitHub, or username, password`_
* `Single sign-on with LDAP`_

Single sign-on with Google, GitHub, or username, password
-------------------------------------------------------------

To allow users to sign in with an ElasticBox username and password, turn on that option under Authentication in the admin console.

Do the same and turn on Google or GitHub to let users sign in with those credentials without having to create an account exclusively for ElasticBox. When they sign in, ElasticBox provisions an account based on their Google or GitHub username.

Single Sign-On with LDAP
----------------------------------------------------

Enable LDAP in ElasticBox to let users log in using credentials managed in OpenLDAP or Windows Active Directory.

Add LDAP sources in ElasticBox to match the structure of LDAP in your organization. For example, if there are several LDAP servers that replicate the service, you may want to add an LDAP source for each. If users are spread across multiple organizational units, you may want an LDAP source for each unit. Or, to limit access to users in specific groups, you may want to add an LDAP source for each group.

When users sign in to ElasticBox with their LDAP credentials, we don’t store their passwords. The login session passes on their credentials to each LDAP source defined in ElasticBox. The LDAP server looks the user up by their username or Use Principal Name (UPN), typically in the **yourname@example.com** format. The server responds with an authorized or unauthorized request. If authorized, we grant the user access in ElasticBox. Else, we deny access.

**In this article:**

* `Setting up LDAP in ElasticBox`_
* `Syncing with LDAP groups`_
* `Giving LDAP accounts admin access`_

Setting Up LDAP in ElasticBox
--------------------------------

**Steps**

1. Sign in to ElasticBox as the `default administrator </../documentation/getting-started/enterprise-setup-onboarding/#enterprise-setup>`_.
2. From the user menu drop-down on the top right, select **Admin Console**.
3. Under Authentication, enable LDAP by turning it on.
4. For each LDAP source, provide information to connect and query the LDAP service when a user tries to log in.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/adminconsole/admin-console-authenticate-users-with-ldap.png" alt="Authenticate Users via LDAP">
		</div>

	* **Host**. Required. The LDAP server hostname as a fully qualified domain name or valid IP address. Examples include **ldap://10.0.128**, **ldaps://10.0.128**, **ldap://directory.companyname.com**, or **ldap://ldap.elasticbox.com**. Specify a port if different than 389. By default, we connect to the service using port 389.
	* **Base DN**. Optional. The fully qualified name of the organizational unit. For example, OU=Accounts,DC=ad,DC=elasticbox,DC=com. If empty, we start the lookup at the root level.
	* **Group DN**. Optional. The group of users who can log in. For example, CN=MyGroup,OU=Groups,DC=ad,DC=elasticbox,DC=com. Add a new LDAP source to specify another group.
	* **Email Field Name**. Typically called **mail**, it's the name of the email field by which to lookup users. It's not required for Active Directory.
	* **Domain Search User**. The LDAP service account that performs the user lookups. Give a fully qualified name, such as CN=integration tests,OU=Service,OU=Accounts,DC=ad,DC=elasticbox,DC=com. For Active Directory, you can also use the domain\username format.
	* **Domain Search Password**. Password for the LDAP service account that looks up users who try to log in.
		**Note**: Provide the search user and search password fields if connecting to an LDAP service that does not support public queries. Without the settings, LDAP group sync won't work and users can't sign up in ElasticBox.

5. Click **Add LDAP Source** to set up the LDAP connection.

Syncing with LDAP Groups
----------------------------

LDAP groups get automatic access to team workspaces in ElasticBox when you enable syncing with those groups. Through the ElasticBox web or API interface, you can directly add LDAP groups as members of a workspace instead of searching and adding them one by one.

.. raw:: html

	<div class="doc-image padding-1x">
		<img class="img-responsive" src="/../assets/img/docs/workspaces/teamworkspaces-addldapgroups.png" alt="Add LDAP Groups to Team Workspaces">
	</div>

This gives your developers, operations engineers, or IT admins access to the same deployment assets to do their part in automating with necessary access levels. Follow these steps to sync with LDAP groups.

**Steps**

1. Sign in as the `default administrator </../documentation/getting-started/enterprise-setup-onboarding/#enterprise-setup>`_.
2. From the user menu drop-down on the top right, select **Admin Console**.
3. Under Authentication, make sure LDAP is on and `set up with at least one source </../documentation/managing-your-organization/user-authentication/#setting-up-ldap-in-elasticbox>`_.
4. Turn on **Enable Group Sync**.

	.. raw:: html

		<div class="doc-image padding-1x">
			<img class="img-responsive" src="/../assets/img/docs/adminconsole/admin-console-enable-ldapgroupsync.png" alt="Sync with LDAP Groups">
		</div>

5. Click Sync Groups to start syncing.

	**Note**: By default, we sync every 24 hours to get the latest group updates. To sync at any other time, click **Sync Groups**. If a group member is deleted or moved out, they no longer have access to ElasticBox workspaces and won't be able to log in.

Giving LDAP Accounts Admin Access
-------------------------------------

As good practice, you should give an LDAP user in your organization administrative access to ElasticBox and set aside the default administrator account to use in case of emergency. After you set up LDAP, give the LDAP user admin access as follows.

**Steps**

1. Sign in to ElasticBox as the LDAP user. This registers the user in ElasticBox with a personal workspace.
2. Log out and log back in as the default administrator.
3. Make the LDAP user an administrator. From here on, use that LDAP user account to manage ElasticBox.



