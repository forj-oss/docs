..    include:: <isonum.txt>

.. _redstone-blueprint:

Redstone
========
.. image:: /img/redstone.png

Summary
-------
If you want to develop like the OpenStack |reg| [1]_ project, the "Redstone" :term:`blueprint` is what you are after.

Learn more on the processes that are used by the OpenStack project here:

* `How to contribute to OpenStack <https://wiki.openstack.org/wiki/How_To_Contribute>`_
* `OpenStack infrastructure team <https://wiki.openstack.org/wiki/InfraTeam>`_

Check this tutorial which shows Redstone in action:

|ImageLink|_

.. |ImageLink| image:: /img/redstone.tutorial.jpg
.. _ImageLink: https://www.youtube.com/watch?v=IUwMFzOALmU

Tools and features
------------------
Very high level, the Redstone flow looks like this:

.. image:: /img/redstone.gif

In its current version, the Redstone blueprint contains the following features:

* Puppet master automation, create new modules and promote them in the :term:`forj-config` project.
* Gerrit/git services for revision control and change management.
	* Login with Launchpad openid, use `www.launchpad.net <http://www.launchpad.net>`_ to setup your email account for authentication.
	* Create open commits that can be reviewed with the team and gated for change before they hit master.
	* Tag and release changes when they are ready for testing or production.
	* Mark commits as work in progress or abandon revert old changes.
	* Create new gerrit projects and acl’s managed from source in forj-config project.
* Jenkins/Zuul integration
	* Create zuul macros to automate common build task, or re-use pre-existing openstack macros included in the pre-configured forj-config project.
	* Create automatic Jenkins jobs so that your team no longer manages Jenkins setup on Jenkins master, all configuration is managed as source in forj-config git project.
	* Gate changes and execute automated test as soon as changes make it to gerrit/git.
	* Promote changes based on gerrit events for tagging or comments made during reviews.
	* Login with launchpad openid
	* Zuul status: see what changes are being automatically tested, the progress and the status from one interface
* Pastebin integration
	* Collaborate with team mates with code snippets and log pasting for troubleshooting defects.
* Defect tracking
	* Mark gerrit changes with defect id’s to link to your defect system with Launchpad or HP Agile Manager

Project Management
------------------

You can manage project using Maestro user interface, or by editing the "forj-config" GIT repository (through Gerrit). 

Adding a new project
********************

In the Redstone blueprint, projects are managed by code, exactly like the OpenStack infrastructure project. This code, which sits in the forj-config Gerrit repository describes a project and is used to provision the entire chain associated to a project (Gerrit, Jenkins, Zuul).

With Maestro User Interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	* Enter the maestro UI and sign-in if needed, so you become the administrator and can add projects. The first user who logs becomes administrator and can add other administrators. Only administrator can create projects.
	* Connect to Gerrit. The first user who logs in to Gerrit becomes administrator
	* As a Gerrit administrator, add yourself to the "forj-config" group. This allows you to review and approve changes in the forj-config project
	* Go to the projects tab then add projects icon, finally set the name for you project and wait ~15 minutes until the change is propagated to gerrit (this uses Puppet mechanisms)
	* Go to Gerrit and approve (+2) or reject (-2) the project creation change
	* Once Puppet has done its work (up to 30 minutes), the new project is fully created and configured in Gerrit, Jenkins and Zuul

.. note::
	You can force "Puppet Apply" and speed up the process through the Jenkins "puppet-apply-all-nodes" job, or directly in a terminal session on the Maestro / Puppet master system


Using "Redstone" mechanisms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You have full control of project creation by checking out the forj-config project and modifying the files that you need.

.. note::
	In the "Redstone" blueprint, everything is threated as code. You must go through Gerrit mechanisms to edit project configuration. Otherwise, your changes will be dismissed the next time the puppet master runs.

* Check out the forj-config project
	Each forge has its own forj-config project. Make sure your ssh keys are added to the gerrit server. To approve a change, make sure your user account is a member of the forj-core group.

* Assigning someone to be a verifier for forj-config project
	When there is no jenkins/zuul functionality on the forj-config project, it's sometimes necessary to verify changes manually. This can be performed by updating the group membership for Continuous Integration Tools group in gerrit.

* add an acl file (to the acls/production directory below) by using cookiecutter.config as an example found in the below path (i.e. make a copy)::

	[forj-config.git] / modules / runtime_project / files / gerrit / acls / templates /  cookiecutter.config

* place cookiecutter.config in directory (projectname should match the name of the project): (note: change the 'cookiecutter-core' to whatever group name you would like to use in your project: example: my-new-proj1-core)::

	[forj-config.git] / modules / runtime_project / files / gerrit / acls / production / projectname.config

* to create project edit file::

	[forj-config.git] / modules / runtime_project / templates / gerrit / config / production / review.projects.yaml.erb

* push changes to your gerrit repo:

	.. sourcecode:: console

		$ git add <new-project-acl-file>
		$ git add review.projects.yaml.erb
		$ git commit -m "my new project"
		$ git push 

* To learn more on how to configure yaml, see `jeepb <http://ci.openstack.org/jeepyb.html>`_ docs.
* you can migrate public projects with the upstream option.
* Projects that are created in gerrit currently have no approach for deletion, but these can be removed from normal users view through acl changes. For more info, please refer to : `rename project <http://ci.openstack.org/gerrit.html#renaming-a-project>`_ or remove project


Adding a new jenkins job and configure Zuul for a given project in gerrit
*************************************************************************
Zuul configuration consist of 4 basic parts.

1. update **hieradata** to include any new templates that will be used for the job in **runtime_project/files/hiera/hieradata/Debian/nodetype/ci.yaml**

	* this is only needed if you need a new compiler option, or new tool that will not exist on the build server.

	* configure in the following section ci-node -> class cdk_project::jenkins -> job_builder_configs. 

	Example:

		.. sourcecode:: yaml

			cdk_project::jenkins::job_builder_configs:
				- 'tutorials.yaml'
				- '<new_job_template_name>.yaml'

2. configure the new template into **runtime_project/templates/jenkins_job_builder/config/**

	* a pre-existing template file can be used to describe the builders for the job, or a new one can be created

	* pre-existing macros can be found in runtime_project/files/jenkins_job_builder/config/macros.yaml

3. update layout.yaml in **runtime_project/files/zuul/config/production/layout.yaml**

	* the projects section should be updated with the new project and gates, along with jobs that will be executed from projects.yaml, example:

	.. sourcecode:: yaml

		projects:
		 - name: tutorials
		   check:
		     - tutorials-flake8
		   gate:
		     - tutorials-flake8
		   post:
		     - puppet-apply-all-nodes
		   release:
		     - tutorials-flake8


4. add the project section to **runtime_project/files/jenkins_job_builder/config/projects.yaml**

	* this will define the jobs to be created in jenkins, job names will be mapped to buiders by zuul. The "name" must match the job-template layout file (line 2 in the jenkins_job_builder file), and the "git_project" must match with the name of your project in gerrit.

	.. sourcecode:: yaml

		projects:
		   name: tutorials
		   git_project: tutorials
		   branch: master
		   jobs:
		    - '{name}-flake8'
		    - '{name}-<new_job_name>'

Once this is done, you will need to push the changes to gerrit, verify and submit. Next the eroplus box will need to run puppet cycle, or puppet agent -t to get the new runtime_project udpates. Finally the ci server will need to run a puppet cycle or puppet agent -t so that the job builder can setup the job.

.. Note:: More info on zuul: `http://ci.openstack.org/zuul <http://ci.openstack.org/zuul>`_


Remove a project in gerrit
**************************

* Stop gerrit:

	.. sourcecode:: console

		$ sudo service gerrit stop

* start the gsql client on local admin bash shell:

	.. sourcecode:: console

		$ java -jar /home/gerrit2/review_site/bin/gerrit.war gsql -d /home/gerrit2/review_site

* remove entries from table account_project_watches

	.. sourcecode:: sql

		select * from account_project_watches;
		delete from account_project_watches where project_name = 'tutorials-2'
		delete changes
		select * from changes where dest_project_name = 'tutorials-2';
		delete from changes where dest_project_name = 'tutorials-2';

* Remove the repo from disk.

	.. sourcecode:: console

		$ rm -rf /var/lib/git/tutorials-2.git
		$ rm -rf /home/gerrit2/review_site/git/tutorials-2.git/

.. Note:: this should be done on all replicas

* Start gerrit back up

	.. sourcecode:: console

		$ service gerrit start


User management
---------------

In the Redstone blueprint, the first user who authenticates to Gerrit and Jenkins become administrator. Then, it is the role of the administrator to add users in the respective tools and projects.

Configure Email Notifications
-----------------------------

In order to gerrit send email notifications you need to configure it first.

* First you need to have at least one project configured in gerrit, and have a forj-config clone git repository.
* Have a external MTA account, in this example we will use sendgrid_. This service has a free account option if you want to try it.
* In the path **forj-config/modules/runtime_project/files/hiera/layouts/** you need to make the following changes in the file **maestro.yaml**:

.. _sendgrid: http://sendgrid.com/

    .. sourcecode:: yaml

          classes:
            - ....
            - exim_config

          exim_config::smarthost: 'smtp.sendgrid.net'
          exim_config::port: '587 byname'
          exim_config::smtp_require_auth: true
          exim_config::smtp_username: 'YOUR_USER'
          exim_config::smtp_password: 'YOUR_PASSWORD'
          exim_config::relay_from_hosts:
            - '127.0.0.1'
            - "%{::exim_config::utils::review_ip}"

          sysadmin_config::manage_servers::iptables_public_tcp_ports:
            - 25
            - ...

* In the same path as above, modify the file **review.yaml** adding the following lines:

    .. sourcecode:: yaml

        classes:
          - exim_config::utils
          - ...

        gerrit_config::smtpserver:      "%{::exim_config::utils::maestro_ip}"
        gerrit_config::sendemail_from:  'YOUR_SENDFROM_EMAIL_NAME'

Email Troubleshooting
---------------------

If you are having problems with this configuration, you can try these steps to find where the problem is.

1. Check if gerrit was configured correctly in Review server:
    1. Check gerrit's config file located in **/home/gerrit2/review_site/etc/gerrit.config** you should have something similar:

    .. sourcecode:: yaml

            [sendemail]
            smtpServer = 10.0.0.90
            from = robot <robot@my.com>

    2. A common problem is an empty IP address, just make sure that **exim_config::utils** in classes is located **before** gerrit class.
    3. Check if the port 25 is open on maestro server:

    .. sourcecode:: shell

            $ nc -v 10.0.0.90 smtp
            Connection to 10.0.0.90 25 port [tcp/smtp] succeeded!
            220 maestro.v5.dev.forj.io ESMTP Exim 4.76 Fri, 18 Jul 2014 22:00:05 +0000

    4. Make sure that you have at least 2 users registered in gerrit before attempting to test it.
    5. While you generate the email event in gerrit, keep monitoring log file: **tail -f /home/gerrit2/review_site/logs/error_log**, you should see if the mail was attempted to sent.

2. Check if exim was correctly configured in maestro.
    1. Try to send a test email to check if the external smtp server was configured correctly:

     .. sourcecode:: shell

        $ echo "Test email " | mail -s "test external" <email_address@my.com>

    2. If you didn't receive the mail you can test changing the parameters manually in exim config file

     .. sourcecode:: shell

        $ service exim4 stop
        $ vim /etc/exim4/exim4.conf
        $ service exim4 start

    3. As a alternative you can use a terminal email client to make further tests.

     .. sourcecode:: shell

        $ apt-get install mutt
        $ vim ~/.muttrc

     and add the following line:

     .. sourcecode:: shell

        set smtp_url = "smtp://smtp.sendgrid.net:587"

     try to send a test email

     .. sourcecode:: shell

       echo "Test email " | mutt -s "test external" email_address@my.com

    4. You can view exim logs in the following location: **/var/log/exim4/mainlog**

* Allow puppet to run at least twice on both maestro and review nodes in order to see your changes show up.

Install SSL Certificates
---------------------

The Redstone blueprint will install a custom certificate that is digitally signed by a self signed certificate registry located on maestro.  This is managed by the cacerts puppet module from the forj-oss/maestro repository.  This however means that developers and end-users that access the review and ci nodes for your forge will be warned and prompted to continue navigation to the site because these certificates are not typically trusted by the browser.  In this section we will describe the process you can follow for configuring a digitally signed certificate from a certificate authority that would more commonly be trusted by your browser.  This would replace the automated self signed certificates that are auto generated by maestro.



.. _redstone-blueprint-faq:

FAQ
---
... How do I create a new project?

   Creating a new project on a redstone forge means creating a new Gerrit repository.
   We use the CI workflow of the forge itself to manage the project creation process.
   Configuration files are modified and updated to provide the administrator of 
   the forge an oportunity to review the commit. Currently we do not provide 
   automatic review option, but one could be setup using zuul gate triggers.

... re-trigger the verification for project create change request?

   If your forge did not trigger a verification check for the project creation 
   request, it is possible to re-trigger the request on the change request.
   Go to the change request and add a new comment.  Make the comment text say:
   'recheck no bug'. This should trigger a zuul gate check for the change request.

... approve a new project creation request on gerrit?

    First you must be the administrator of your forge or contact and the administrator
    of the forge you will try to access.  The approving user must be added to the 
    group, forj-core.  This can be done in Gerrit from the Admin->Groups menu by the 
    Gerrit administrator.  Once done, the user added can then administer
    approvals by adding +2 for Code Review and +1 for Approved on the change request.

... change the group that approves changes for forj-config on gerrit?

    Approval permissions for groups is managed by the forj-config acl's file.
    This can be updated with a change request update to the forj-config source on the
    file : 
    [forj-config.git]/modules/runtime_project/files/gerrit/acls/production/forj-config.config
    
    Change the group forj-core to a new group name.  If the group does not exist
    a new one will be created.


.. [1] The OpenStack Word Mark and OpenStack Logo are either registered trademarks/service marks or trademarks/service marks of the OpenStack Foundation, in the United States and other countries and are used with the OpenStack Foundation's permission. We are not affiliated with, endorsed or sponsored by the OpenStack Foundation, or the OpenStack community.
