.. Global glossary. Terms do not need to be sorted in alphabetical order: they will be sorted automatically.
.. Use :term:`a term`

########
Glossary
########


.. glossary::
	:sorted:

	Forge
		The set of tools - installed by Maestro - by parsing the blueprint. This is the end product, and what the developers will actually use. This process is also used when restoring data to a new forge (safe restore).

	Forj's forge
		Where Forj itself is developed. `review.forj.io <http://review.forj.io/>`_. Most of the code hosted there is also published on Github automatically. 

	forj-config
		Special GIT project hosted on a forge which holds the configuration of the forge. 

	Maestro
		Maestro manages all the tools from a forge. For forge users, it provides a web based user interface easily access all the tools that are used by a project. You also go to Maestro UI to register, so that an administrator can later provision your privileges. Forge administrators can manage projects and users and administrate the forge (status, backup/restore, :term:`take home`).

	Catalog
		The list of :term:`blueprint` available. The public blueprints are hosted on Forj's forge.

	Portal
		Available at `portal.forj.io <http://portal.forj.io/>`_, the portal is a mean to browse the catalog of forges and provision forges in few clicks. As an enterprise, you can replicate such portal and offer "forges as a service". Forj's portal uses `HP CSA product <http://hp.com/go/csa>`_.

	Blueprint
		Source code which tells Maestro how to install, integrate, operate, update, backup, restore, relocate a forge. A blueprint describes a forge programatically. 

	Take home
	Relocation
	Relocate
		Process initiated by Maestro to move all the systems of a forge from one location (public cloud, private cloud or on premises systems) to another.

	