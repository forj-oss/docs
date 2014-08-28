Introduction
============

Organization
------------
Forj is combination of 2 components:

* :term:`Maestro`. Maestro's code is managed in :term:`Forj's forge`, and published to Github under `forj/maestro <https://github.com/forj/maestro>`_.
* :term:`Blueprint` (s). Read by Maestro to install a :term:`forge`. Those blueprints are exposed in Forj's catalog, developed in :term:`Forj's forge`, and published to Github under `forj <https://github.com/forj-oss/>`_. There is one GIT repository per blueprint. 


Philosophy
----------

Forj is **vendor-agnostic**. The goal is to provide integrated software development tools which solve a problem. The tools can be all Open Source, a mix of Open Source and proprietary tools, or a set of proprietary tools. As long as a tool has a way to be installed or integrated with other tools, Forj will have a way to make it part of the :term:`Forge`. In other words, it is possible to integrate tools hosted in the cloud with tools installed on your own servers to create a :term:`forge`.

Forj is **destination-agnostic**. The :term:`forge` can be installed on a public or a private cloud or on premises. Forj leverages the `fog library <http://fog.io>`_ to be able to provision systems on many different  clouds and thus avoiding vendor lock-in. A great benefit is that you can :term:`relocate` a forge from a public cloud to a private cloud or from a cloud provider to another. 

