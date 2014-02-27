Openstack
=========

Summary
-------
If you want to develop like the Openstack project, the "a la Openstack" :term:`score` is what you are after.

Learn more on the processes that are used by the Openstack project here:

* `How to contribute to Openstack <https://wiki.openstack.org/wiki/How_To_Contribute>`_
* `Openstack infrastructure team <https://wiki.openstack.org/wiki/InfraTeam>`_

Tools and features
------------------
In its current version, the score contains the following features:

* Puppet master automation, create new modules and promote them in the :term:`forj-config` project.
* Gerrit/git services for revision control and change management.
	* Login with google openid, use www.google.com to setup your email account for authentication.
	* Create open commits that can be reviewed with the team and gated for change before they hit master.
	* Tag and release changes when they are ready for testing or production.
	* Mark commits as work in progress or abandon revert old changes.
	* Create new gerrit projects and acl’s managed from source in forj-config project.
* Jenkins/Zuul integration
	* Create zuul macros to automate common build task, or re-use pre-existing openstack macros included in the pre-configured forj-config project.
	* Create automatic Jenkins jobs so that your team no longer manages Jenkins setup on Jenkins master, all configuration is managed as source in forj-config git project.
	* Gate changes and execute automated test as soon as changes make it to gerrit/git.
	* Promote changes based on gerrit events for tagging or comments made during reviews.
	* Login with google openid
	* Zuul status: see what changes are being automatically tested, the progress and the status from one interface
* Pastebin integration
	* Collaborate with team mates with code snippets and log pasting for troubleshooting defects.
* Defect tracking
	* Mark gerrit changes with defect id’s to link to your defect system with Launchpad or HP Agile Manager
