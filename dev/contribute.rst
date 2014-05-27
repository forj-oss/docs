 .. include:: /links.txt

Contribute
==========

Governance Model
****************

Organization
------------
The Forj project is an open community of developers, which is governed by the Apache Software License v2.0. It is comprised of Forj development team members, contributors from HP, and external contributors. Since the community structure is evolving, in this initial stage, all components will have oversight by the Forj core team. 

Core team
---------
The Forj core team is composed by the following individuals:

+------------------------+-----------------------+----------------+
| Role                   | Name                  | Nickname (IRC) |
+========================+=======================+================+
| Program Management     | Rafael Garcia         | RafGar         |
+------------------------+-----------------------+----------------+
| Product Owner          | Olivier Jacques       | osanchez       |
+------------------------+-----------------------+----------------+
| Overall Architect      | Edward Raigosa        | wenlock        |
+------------------------+-----------------------+----------------+
| Lead                   | Miguel Quintero       | miqui          |
+------------------------+-----------------------+----------------+
| Lead                   | Christophe Larsonneur | chrissss       |
+------------------------+-----------------------+----------------+
| Lead                   | Oscar Romero          | homeless       |
+------------------------+-----------------------+----------------+
| Community Manager      | Arne Luehrs           | che-arne       |
+------------------------+-----------------------+----------------+

We encourage participation by anyone who wants to collaborate on the direction of the Forj Project. The best way to get involved is to join the community and take an active role as a contributor. As the community grows, and public developers take on more leadership in contributing to components, we hope to extend maintainer roles to external contributors from the Forj community at large.

.. note::
	If you are proposing large code changes or major features, we ask that you discuss it with us first in one of the `IRC`_ or `mailing list`_ - this ensures any major investment of time on your part is aligned with the overall direction of the Project and that the community can discuss and build consensus before you begin work.

.. _communication-and-process:
Communication and Process
-------------------------
The Project uses well known tools and processes to make contributing and communicating straightforward.

These resources include this website, our own review system based on Gerrit, GitHub to publish code, a bug tracker, `mailing list`_ and an `IRC`_ channel. 

A mechanism inspired by the well known Linux :ref:`"Developer Certificate of Origin (DCO)" <dco>` is used to sign off contributions. 

Everyone contributing to the Forj Project or participating on the `IRC`_, `mailing list`_ or other resources provided by the Project must abide by the Terms of Use and Community Guidelines. While creative ideas and respectful debates on topics are encouraged, the Forj Project reserves the right to act as needed to protect the integrity of the Project, including the removal of any posts deemed inappropriate or offensive.

Community members should note that the information submitted via the `IRC`_, `mailing list`_ or any other communications, resources of the Forj Project will immediately become public information.

.. _how-do-I-contribute:
How do I contribute?
********************

1. Join the Community: 
Joining allows you to contribute code, participate in `IRC`_ and `mailing list`_, report and track bugs and receive updates on the latest Forj developments.

2. Decide Your Purpose: 
Do you want to add a feature to Forj, contribute to a blueprint or enhance the documentation?

3. Get Familiar with the Components: 
Once you determined your activity, check out our repos on `Forj's Gerrit`_ or `GitHub`_ to review our components and READ ME files. 

4. Discuss Your Idea: 
Before you even start updating components, you should post your ideas on the `IRC`_ or `mailing list`_. You can find out if someone else is working on a similar idea or community members can help you refine your concept.

5. Review the Governance Model: 
Follow our process for how to get your contribution accepted as detailed under our Governance Model.


.. _accept-contributions:
How We Accept Contributions
***************************

The Contribution Process
------------------------

* Take a look at our `issue list`_ and consider submitting a patch. You may have a look at our "fruits" issues (from the low hanging kind) to get started.
* For big changes, the contributor communicates with the Project via mailing lists or `IRC`_ or `Issue List`_ to get feedback before submitting code
* Contributor adds the Forj :ref:`"DCO signoff" <dco>` to each commit message during development - see the link for format and meaning
* Contributor uses the Gerrit workflow to submit the code for review
* Maintainer conducts code review, verifies :ref:`"DCO signoff" <dco>`, runs tests and asks for adjustments from contributor as necessary
* Maintainer merges the commits into the repo

Forj's code development process mimics the one used by the OpenStack project. If you are an OpenStack contributor, you will feel at home.

.. figure:: /img/contribute_code.png

For more information on this process, please refer to the `Openstack's Gerrit workflow <https://wiki.openstack.org/wiki/Gerrit_Workflow>`_.

Useful links:

* `Issue list`_
* `Forj's Gerrit`_
* `Community`_

Documentation is also an open source project. The sources are available from the main Forj repository on Github. We encourage you to make improvements, whether big or small, to this documentation.

.. figure:: /img/contribute_doc.png

Useful links:

* `Forj's documentation repository`_
* `Sphinx`_ document generator

Forj's user and admin documentation (http://docs.forj.io/) is hosted on "readthedocs.org" (http://read-the-docs.readthedocs.org/) at this address: https://readthedocs.org/projects/forj/
The documentation uses "RST" markup language and is generated with Sphinx http://sphinx-doc.org/

To contribute to the documentation

* Clone the docs repository from github: http://github/forj/docs
* Setup your sphinx environment on your PC: http://sphinx-doc.org/
* Edit the doc
* test it with "make html"
* Open a pull request and interact with the core team
* Once reviewed, the changes are merged by the core team in the Github repository
* The documentation gets refreshed automatically thanks to a web hook


Criteria before submitting a contribution
-----------------------------------------

* Contributor has verified that their changes do not break any of the builds
* Contributor has provided or updated unit tests, if there is an existing unit test structure for any of the components affected
* If there is no unit test structure, the contributor has thoroughly tested their changes manually, and can describe the results
* Code is in the style of the code that surrounds it


During Review the Maintainer Will
---------------------------------

* Look to see that your signoff is in all your commit messages, including format, and presence of real name and real email address
* If you are someone entirely new to the Project, they may get in touch with you via the contact information you have provided
* If there are anomalies such as inconsistent name or email address between signoffs, they may ask you to clarify
* This process may take some time, since we may conduct testing, and there may be concurrent activity which must be checked for merge conflicts, architectural issues, etc.

After the merge
---------------

* Your commits will be merged onto the master branch
* The maintainer's identity who accepted your pull request will be recorded in the merge
* Congrats, your are now a Forj contributor!
