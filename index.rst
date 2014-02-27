.. image:: /img/forj_logo.png
.. include:: <isonum.txt>

Welcome to Forj!
================
Forj is a mean for software developers to get a continuous integration (CI) / delivery (CD) tool stack which is working out of the box, and can be installed anywhere in minutes. That kind of environment is often refered to as a `forge <http://en.wikipedia.org/wiki/Forge_(software)>`_, a collaboration platform for software developers.

Forj is Open Source (see the :doc:`/license`) and provides:

* A collection of pre-integrated continuous integration and delivery tool stacks called blueprints
   * Forj has currently one continuous integration stack which mimics the Openstack |trade| Open Source project. We look forward to build a Forj community to add many continuous integration stacks to the catalog.
* An orchestration and administration component called *Maestro* which provides a uniform way to provision projects and users as well as manage the forge (backup/restore, monitoring, updates).

In summary, it goes like this:

.. figure:: /img/intro.png

   Forj's flow

Still here? Let's dig further.
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

