.. include:: /links.txt

.. _apaas:

aPaas
=======

Application platform as a service (aPaaS) is a cloud service that offers development and deployment environments for application services.

The following blueprints:

* redstone (aPaas details for this blueprint can be found under: redstone/puppet/modules/runtime_project/files/apaas)

include a CI/CD node with the CLIs necessary to interact with the following aPaas environments:

* `Hewlett-Pacakard Helion Development Environment`_
* `ActiveState Stackato`_
* Any vendor that supports `CloudFoundry`_


The overall aPaas integration scenario is for a blueprint to include a node (OS depends on the aPaas vendor's CLI OS support - almost all run on linux/Mac OSX) that
will execute the CLIs to interact with their environment. More support for aPaas vendors can be easily added by simply installing on the CI/CD node the required
CLIs via the blueprint's orchestration mechanism.
