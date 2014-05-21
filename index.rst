.. image:: /img/forj_logo.png
.. include:: <isonum.txt>

Forj documentation
==================
Forj provides software developers with a rapid, flexible and open continuous integration (CI)/ continuous delivery (CD) development platform. 
This environment is often referred to as a software forge, even though, at the time the name was invented, the notion of CI and CD was not as prevalent.

Forj provides:

* A collection of pre-integrated continuous integration and delivery tool stacks called blueprints
   * Forj has currently one continuous integration stack which mimics the OpenStack |trade| Open Source project. We look forward to build a Forj community to add many continuous integration stacks to the catalog.
* An orchestration and administration component called *Maestro* which provides a uniform way to provision projects and users as well as manage the forge (backup/restore, monitoring, updates).

See the `Forj web site <http://www.forj.io>`_ for more information, use cases and flows.

.. image:: /img/forj_io.jpg

Documentation
=============
The documentation has 2 parts:

* The "user guide" section is relevant for Forj users as well as forge administrators and users.
* The "developer guide" is for anyone who wants to contribute to the Forj project.

User guide
----------

.. toctree::
   :maxdepth: 1
   :numbered:

   user/intro
   user/install
   user/admin
   user/kits/openstack
   help
   glossary
   license

Developer guide
---------------

.. toctree::
   :maxdepth: 1
   :numbered:

   dev/intro
   dev/contribute
   dev/architecture
   dev/devenv
   dev/faq


Indices and tables
==================

* :ref:`genindex`
* :ref:`search`

