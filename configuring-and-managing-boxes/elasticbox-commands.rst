ElasticBox Commands
********************************

The Set and Config commands are useful to generate dynamic values for the variables in your box at deploy time. The notify command is useful to allow other instances to react to those changes.

* `Set command`_
* `Config command`_
* `Notify command`_

Set Command
-----------------------

Use the command to programmatically set the value of variables you don't know beforehand, like authentication keys. Do this in the parent box to set the values of its variables or of variables in the child box scripts.

Syntax
````````````

To set the value of a parent box variable:

.. raw:: html

	<pre>
	elasticbox set &#60;variable_name&#62; &#60;variable_value&#62; -box &#60;box_name&#62;
	</pre>

To set the value of a child box variable in the parent:

.. raw:: html

	<pre>
	elasticbox set &#60;childbox_variable_name&#62;.&#60;variable_name&#62; &#60;variable_value&#62; -box &#60;box_name&#62;
	</pre>

+-------------------------+------------------+--------------------------------------------------+
| Parameter               | Type             | Description                                      |
+=========================+==================+==================================================+
| box                     | String           | Optional. Box name to run the command from SSHing|
|                         |                  | into the instance of the deployed parent box. The|
|                         |                  | parameter can include nested child box           |
|                         |                  | references.                                      |
+-------------------------+------------------+--------------------------------------------------+

Example
```````````

This event script uses the Set command to generate a dynamic string based on the password, address, and username variables. At deploy time, when the script is executed, the resulting value stored in MONGODB_CONNECTION_STRING is used to log in to the MongoDB instance.

 .. raw:: html

	<pre>
	elasticbox set MONGODB_CONNECTION_STRING "mongodb://$user:$pass@$address.public:27017/elasticbox?safe=true"
	</pre>

Config Command
-----------------------

Use the Config command on file variables to generate dynamic values when deploying. A file variable usually has additional scripts to configure your application. For ElasticBox to know what dynamic values to return, include variables or binding references in the files. Then call the file from an event using the Config command.

To get the file on to the virtual machine, use cURL or Wget like commands. When deploying, ElasticBox replaces the variables or binding references with configuration values, executes the file, and stores the result in an output file.

Syntax
`````````````
.. raw:: html

	<pre>
	elasticbox config -i &#60;input_file&#62; -o &#60;output_file&#62; -box &#60;box_name&#62; -t &#60;template_engine&#62;
	</pre>

+-------------------------+------------------+--------------------------------------------------+
| Parameter               | Type             | Description                                      |
+=========================+==================+==================================================+
| i                       | String           | Optional. Input file to run the Config command.  |
|                         |                  | When you don’t specify, the command reads data   |
|                         |                  | from standard input.                             |
+-------------------------+------------------+--------------------------------------------------+
| o                       | String           | Optional. Output file that stores the            |
|                         |                  | configuration values. When you don’t specify, the|
|                         |                  | command directs the data to standard output.     |
+-------------------------+------------------+--------------------------------------------------+
| box                     | String           | Optional. Box name to run the command from SSHing|
|                         |                  | into the instance of the deployed parent box.    |
+-------------------------+------------------+--------------------------------------------------+
| t                       | String           | Optional. Template engine type, which is either  |
|                         |                  | Jinja2 or Velocity. If you set this flag, the    |
|                         |                  | command gives the values of the variables that   |
|                         |                  | match the template type in the file.  If you     |
|                         |                  | don't set the flag, the command gives values for |
|                         |                  | variables of both template styles.               |
+-------------------------+------------------+--------------------------------------------------+

Example
`````````````

Suppose you upload a CHEF_DEFAULT_RB file to the Chef box. This file has a Rails cookbook script that refers to three Chef box variables (folder, RAILS_APP, and CLONE_URL).

**Here are the file contents:**

.. raw:: html

	<pre>
	# Default Recipe

	include_recipe "git"

	include_recipe "nodejs"
	include_recipe "sqlite"

	#if (${RAILS_APP} != '')
	application 'web_app' do
  		path '${folder}/${RAILS_APP}'
  		owner 'root'
  		group 'root'
  		repository '${CLONE_URL}'

  		rails do
   			bundler true
   			precompile_assets true
   			database do
   			adapter "sqlite3"
    			database "db/${RAILS_APP}.sqlite3"
   			end
  		end
	end
	#end
	</pre>

In order for ElasticBox to act on this file at deploy time, use cURL or WGET commands in an event script on the Chef box to download the file into the virtual machine. Then, pass the file through the Config command in the event script so that ElasticBox executes the Chef box variables in it.

**Here the Config Command is run on the file:**

.. raw:: html

	<pre>
	curl -ks ${CHEF_DEFAULT_RB} | elasticbox config -o
	cookbooks/${CHEF_COOKBOOK_NAME}/recipes/default.rb
	</pre>

At deploy time, ElasticBox runs the Config command on the CHEF_DEFAULT_RB file, replaces the variables with actual deploying values, and stores the file as default.rb in the specified path on the virtual machine.

Notify Command
-----------------------

Use the command to programmatically send a notification to all instances that bind to the current instance.

Syntax
``````````

To send notification to all instances binding to the current one:

.. raw:: html

	<pre>
	elasticbox notify
	</pre>

Usage
``````````

The most common use is to send a notification after executing one or several elasticbox set commands. The idea is that you can notify instances that are using those variables via bindings

Example
````````````

A common pattern is to have variables that symbolize the state of the instance and use elasticbox notify to specify that status. For example, you could have a REPLICA_SET_READY variable. When the MongoDB replica set is initialized you set it to true in the instance and execute elasticbox notify. All instances binding to the previous one can check the variable REPLICA_SET_READY and execute some configurations if the value is true. If it is not true then it can skip that configuration, and they will be reconfigured (and that code executed) by that instance when the service is ready.



