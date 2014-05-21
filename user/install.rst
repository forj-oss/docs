.. include:: /links.txt

.. _install:

Installation
============

There are 2 ways to provision a forge: through forj command line or the forj portal.

.. _install-cli:

forj command line
-----------------
.. note::
	At the time of this writing, the forj command line has not been released yet.

You will use the forj command line if you want to provision forges on a private cloud or a public cloud of your choice. 
The command line also helps you integrate forge creation and management in your own environment.
Finally, the command line is also how you can create your own :term:`blueprint` or edit an existing one. See the `developer guide`_ for more information on developing for Forj.

forj manpage::
	
	forj boot <blueprint> on <cloud_provider>

	forj command line arguments is an aggregation, in priority order, of
	command line arguments, environment variables, forj.yaml config file

	Arguments (in the form of "yaml name (environment_variable)"):
	- action (FORJ_ACTION):
		- boot: boot a Maestro box and instruct it to provision the blueprint
		- down: delete the Maestro box and all systems installed by the blueprint
	- blueprint (FORJ_BLUEPRINT): name of the blueprint to be provisionned 
	- cloud_provider (FORJ_CLOUD_PROVIDER): cloud provider type to be used to 
		provision the forge. 
		Accepted values:
		- hpcloud: uses HP public cloud http://hpcloud.com/
		- openstack: OpenStack based private or public cloud
		- More to come
	- credentials (FORJ_CLOUD_CREDENTIALS)
		TBD

	Examples:
		TBD


.. _install-portal:

forj portal
-----------
You can use Forj's :term:`portal` to install a forge on HP Public Cloud or an OpenStack compatible public cloud. If you are an enterprise, you can also make forj available through your IT services portal.
As an enterprise, you can replicate such portal and offer "forges as a service". Forj's portal uses `HP CSA product <http://hp.com/go/csa>`_ .

The portal will ask you the credentials of your cloud so that it can provision the forge automatically. If you are not comfortable with this, or want to install forges in a private cloud, you should use :ref:`"forj command line" <install-cli>`. 

.. image:: /img/forj_portal.jpg