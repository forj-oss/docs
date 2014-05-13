FORJ FAQ
----

This is a list of Frequently Asked Questions about Forj.  Feel free to
suggest new entries!

OpenStack Forj, How do I ...
~~~~~~~~

... create a new project?
   Creating a new project on OpenStack Forj means creating a new Gerrit repository.
   We use the CI workflow of the Forj it-self to manage the project creation process.
   Configuration files are modified and updated to provide the administrator of 
   the forj an oportunity to review the commit.   Currently we do not provide 
   automatic review option, but one could be setup using zuul gate triggers.

... re-trigger the verification for project create change request?
   If your Forj did not trigger a verification check for the project creation 
   request, it is possible to re-trigger the request on the change request.
   Go to the change request and add a new comment.  Make the comment text say:
   'recheck no bug'.   This should trigger a zuul gate check for the change request.

... approve a new project creation request on gerrit?
    First you must be the administrator of your forj or contact and the administrator
    of the forj you will try to access.  The approving user must be added to the 
    group, forj-core.  This can be done in Gerrit from the Admin->Groups menu by the 
    administrator of the forj.  Once done, the user added can then administer
    approvals by adding +2 for Code Review and +1 for Approved on the change request.

... change the group that approves changes for forj-config on gerrit?
    Approval permissions for groups is managed by the forj-config acl's file.
    This can be updated with a change request update to the forj-config source on the
    file : 
    [forj-config.git]/modules/runtime_project/files/gerrit/acls/production/forj-config.config
    
    Change the group forj-core to a new group name.  If the group does not exist
    a new one will be created.

Open Entries & Notes
~~~~~~~~