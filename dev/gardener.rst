Gardener
=========
Module in charge of manage configurations, credentials, user-data,... for forj kits, creates and destroys static nodes.


Install Gardener
--------
The installation of gardener.

1. During the creation of master, in the file `10-puppet.pp` the instruction `puppet apply $PUPPET_FLAGS --modulepath=$PUPPET_MODULES /opt/config/production/puppet/manifests/bootstrap_hiera.p` in thee Stage [Main] the file init.pp from gardener is loaded.

2. The file init.pp include the class requirements, this loads the file requirements.pp.

3. The file requirements.pp, look for the requirements and their versions.

Figure below shows the flow of control.


.. figure:: /img/gardener_install.jpg


Verifying Installation and Loading Plug-ins
-------
At the end of the file 10-puppet.pp is executed the command `puppet agent $PUPPET_FLAGS --waitforcert` this will verifies the install and load the plug-ins of puppet (the gardener's plug-ins are also loaded).

At the end of the file 20-instantiate-forj-at-boot.sh is executed the command `puppet agent --debug --verbose $PUPPET_FLAGS --waitforcert`, this will verifies the install and will check the status of the nodes, if any node is listed as not found or not created, it is created.


.. figure:: /img/gardener_verify.jpg


Loading Provider and Credentials
-------
When the plug-ins are loaded, will load the provider and credentials configuration 

1. Load `pinas/loader.rb`.
2. Loader will try to get the provider using `provider.rb`.
3. provider.rb will search for the configuration section clled FOG_RC inside the configuration file `cloud.fog`.
4. Next `compute_pulblic_ip_lookup.rb` will be loaded.
5. This one will check for if the fog_credentials exist if no will create whit `fog_credentials.rb`.
6. fog_credentials will add the feature credentials to yaml and try to load it from the file `cloud.fog`.
7. In case that not found the `compute_pulblic_ip_lookup.rb` will try to load FOG_RC using `pinas/loader.rb`.
8. Load `pinasdns/loader.rb` and will do 2 and 3..
9. load `domain_record_exists.rb` and will try to do 5 and 6 if necessary 7.


.. figure:: /img/gardener_load_provider_and_credentials.jpg


Creating User Data
-------
In `20-instantiate-forj-at-boot.sh` will create the nodes of the blueprint but to do it we will need user-data to create them.

1. Is set a relationship `write-mime-multipart.template.py` before `boothook.template.sh`.
2. Is set a relationship `boothook.template.sh` before `boot-node.template.sh`.
3. Is set a relationship `boot-node.template.sh` before `cloud-config-node.template.yaml`.
4. Is set a relationship `cloud-config-node.template.yaml` before `Exec[create /tmp/mime.txt]`.
5. Is set a relationship `Exec[create /tmp/mime.txt]` before `Exec[dos2unix for /tmp/mime.txt]`.
6. Then the `gardener::srever_up.pp::gen_userdata` will be executed .
7. This wil use `gardener::gen_userdata.pp` execute the files necessary for the generation.
8. `gardener::gen_userdata` will create the `file mime.txt` according to the relationships created before..


.. figure:: /img/gardener_create_userdata.jpg


.. _accept-contributions:
Creating Nodes
-------
This is one of the main functionalities of gardener.

1. `gardener::params.pp` has as one of its attributes compute, which call to `compute.rb`.
2. `compute.rb` will call `actions.rb` method create, while have servers in the template this will be executed.
3. `actions.rb` get the server name using `common.rb` get server_name function.
4. Next, `actions.rb` will check if the server already exists, using  `pinnascompute.rb` server_exist? function.
5. This function uses `common.rb` find_match function to check if exists.
6. If not exist `pinascompute.rb` sill use the method create_server.
7. This method gets parameters from mime.txt and try to create new server.


.. figure:: /img/gardener_node_creation.jpg


Destroying Nodes
-------
this functionality will destroy the nodes created.

maestro::orchestrator::unwindallservers will use `gardener::server_destroy`.

1. This one will use `compute.rb` to destroy the servers using `actions.rb`.
2. To destroy the server will do it one by one according to the server template.
3. To do it first will get the server name using `common.rb` get_servername.
4. Now that it has the server name will use `pinascompute.rb` server destroy method.
5. To destroy it first it recover the server using `common.rb` find_match.
6. And then it just destroy the server.

.. figure:: /img/gardener_node_destroy.jpg


