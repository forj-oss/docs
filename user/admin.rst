Administration
==============

You administer a forge through the Maestro user interface.

.. image:: /img/maestro.jpg

This includes:

* General forge administration (health, backup & restore)
* Project administration
* Tool administration 
* User administration

Tools
-----
The list of tools that you see in the Maestro User Interface depends on the :term:`blueprint`. Each box, which represents a tool exposed can be clicked to open the tool itself.

.. image:: /img/tool.jpg

* Admin: if you are an administrator of the forge, tool which have one will expose an additional "Admin" link, to jump to administration interface of that tool.
* Workload: provides detail on the tool workload, such as CPU, Memory or storage consumption of the tool, as well as tool specific metrics (eg commits / day)
* Backup: tool specific backup and restore operations

Projects
--------
The notion of projects varies from blueprint to blueprint. In the :ref:`"Redstone" <redstone-blueprint>` blueprint, the project creation process from OpenStack infra is automated and exposed with the Maestro UI. 
Please refer to the appropriate section of project management for the blueprint you use.

By default this section is only available for the forge aministrator, but you can change that if you like currently we have 3 leves of access:
 * Administrator (They have access to everything in the forge)
 * Authenticated Users (Limited access to a few features of the forge)
 * Anonymous Users (Only view access to the tools of the forge)

To change the access level for the Projects section you need to modify the file maestro.yaml that is located in:
	/opt/config/production/git/maestro/puppet/modules/hiera/files/hiera/hieradata/Debian/nodetype/maestro.yaml

There change the line that says global_manage_projects under jimador::site for the available options down below
	global_manage_projects: "admin"

Available options
 * "admin" = administrator access only
 * "authenticated" = authenticated users and administrator
 * "anonymous" = everyone

After you do that and save your changes do a puppet run to apply those changes to the config.json file
 puppet agent --test

If you want to see if your change was applied open the config.json file and there you will see the "global_manage_projects" with your new value.

Users
-----
Like projects, users may be managed differently from blueprint to blueprint. 

.. note::
	Implementation is in progress.

Backup/Restore
--------------
Maestro provides backup and restore capabilities for the forge. There is the notion of a forge wide backup/restore process, as well as per tool backup/restore.
The forge wide backup status and restore operation is available to administrators through Maestro UI.

.. note::
	Implementation is in progress. While backups are implemented, the status, and the ability to restore is not exposed in Maestro UI.

Monitoring
----------
Each blueprint can consume monitoring services and expose jauges in the Maestro UI.

.. note::
	Implementation is in progress.
