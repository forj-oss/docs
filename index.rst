.. image:: /img/forj_logo.png
.. include:: <isonum.txt>

Welcome to Forj!
================
Forj provides software developers with a rapid, flexible and open continuous integration (CI)/ continuous delivery (CD) development platform. 
This environment is often referred to as a software forge which provides a collaborative platform for software development.

Forj provides:

* A collection of pre-integrated continuous integration and delivery tool stacks called blueprints
   * Forj has currently one continuous integration stack which mimics the Openstack |trade| Open Source project. We look forward to build a Forj community to add many continuous integration stacks to the catalog.
* An orchestration and administration component called *Maestro* which provides a uniform way to provision projects and users as well as manage the forge (backup/restore, monitoring, updates).

Example of a FORJ blueprint selection flow:

1. Developer selects a 'blueprint' from the FORJ portal. For this example, the developer may select the "A la" Openstack software forge blueprint (development kit). 
   This blueprint includes standard open source tools for configuration management, code review, and continuous integration (automated builds and testing). 
   It also includes the Maestro tool explained above.
2. A 'software forge' is provisioned and hosted on FORJ's public cloud environment.
3. At some point the 'software forge' may be relocated to a separate public could, private cloud or 'on premises' location.


.. image:: /img/forjflow.jpg



The "user guide" section is relevant for Forj users as well as forge administrators and users.
The "developer guide" is for anyone who wants to contribute to the Forj project and become a "Forjer".

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

