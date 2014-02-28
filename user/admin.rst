Administration
==============

You administer a forge through the Maestro user interface. This includes:

* General forge administration (health, backup & restore, :term:`take home`)
* Project administration
* Tool administration 
* User administration

.. note::
	Currently, the Maestro user interface is not completed. The configuration is done through a special project on Gerrit called :term:`forj-config`.

Pre-requisite
-------------

* Checking out the forj-config project
	Steps for how to checkout the forj-config project on a forge. Each forge has its own forj-config project. 
	The following steps can be used to checkout a project but before performing this action, make sure you ssh keys are added to the server. To approve a change, make sure your user account is a member of the forj-core group.

* Assigning someone to be a verifier for forj-config project
	When there is no jenkins/zuul functionality on the forj-config project, it's sometimes necessary to verify changes manually. This can be performed by updating the group membership for Continuous Integration Tools group in gerrit.


Projects
--------

Adding a new project in gerrit
******************************

* add an acl file (to the acls/production directory below) by using cookiecutter.config as an example found in the below path (i.e. make a copy)::

	[forj-config.git] / modules / runtime_project / files / gerrit / acls / templates /  cookiecutter.config

* place cookiecuttern.config in directory (projectname should match the name of the project): (note: change the 'cookiecutter-core' to whatever group name you would like to use in your project: example: my-new-proj1-core)::

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
Zull configuration consist of 4 basic parts.

1. update **site.pp** to include any new templates that will be used for the job

	* this is only needed if you need a new compiler option, or new tool that will not exist on the build server.

	* configure in the following section ci-node -> class cdk_project::jenkins -> job_builder_configs. 

	Example:

		.. sourcecode:: yaml

			class { cdk_project::jenkins:
				vhost_name                        => $node_vhost,
				jenkins_jobs_password             => ,        
				job_builder_configs               => [  'fortify-scan.yaml',
				                                       'pet-clinic.yaml',
				                                       'publish-to-stackato.yaml',
				                                       '<new_job_template_name>.yaml'
				                                     ],
			}

2. configure the new template into **runtime_project/templates/jenkins_job_builder/config/**

	* a pre-existing template file can be used to describe the builders for the job, or a new one can be created

	* pre-existing macros can be found in runtime_project/files/jenkins_job_builder/config/macros.yaml

3. update layout.yaml in **runtime_project/files/zuul/config/production/layout.yaml**

	* the projects section should be updated with the new project and gates, along with jobs that will be executed from projects.yaml, example:

	.. sourcecode:: yaml

		projects:
		 - name: pet-clinic
		   check:
		     - pet-clinic-maven-package
		     - pet-clinic-fortify-scan
		   gate:
		     - pet-clinic-maven-package
		     - pet-clinic-<new_job_name>
		   release:
		     - pet-clinic-publish-to-stackato


4. add the project section to **runtime_project/files/jenkins_job_builder/config/projects.yaml**

	* this will define the jobs to be created in jenkins, job names will be mapped to buiders by zuul. The name must match the job-template layout file (line 2 in the jenkins_job_builder folder).

	.. sourcecode:: yaml

		projects:
		   name: pet-clinic
		   branch: master
		   jobs:
		    - '{name}-maven-package'
		    - '{name}-fortify-scan'
		    - '{name}-publish-to-stackato'
		    - '{name}-<new_job_name>'

Once this is done, you will need to push the changes to gerrit, verify and submit. Next the eroplus box will need to run puppet cycle, or puppet agent -t to get the new runtime_project udpates. Finally the ci server will need to run a puppet cycle or puppet agent -t so that the job builder can setup the job.

.. Note:: More info on zuul: `http://wiki.cdkdev.org/w/index.php/Zuul <http://wiki.cdkdev.org/w/index.php/Zuul>`_


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
		delete from account_project_watches where project_name = 'pet-clinic-2'
		delete changes
		select * from changes where dest_project_name = 'pet-clinic-2';
		delete from changes where dest_project_name = 'pet-clinic-2';

* Remove the repo from disk.

	.. sourcecode:: console

		$ rm -rf /var/lib/git/pet-clinic-2.git
		$ rm -rf /home/gerrit2/review_site/git/pet-clinic-2.git/

.. Note:: this should be done on all replicas

* Start gerrit back up

	.. sourcecode:: console

		$ service gerrit start


Tools
-----
Documentation in progress.

Users
-----
Documentation in progress.