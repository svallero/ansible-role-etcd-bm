etcd role
=========

System wide installation of etcd on bare metal or virtual machine (not containerized).

Supported operating systems: CentoOS 7 and Ubuntu 14.04. 

This role has been specifically developed to be used for the deployment of Calico in the framework of INDIGO-DataCloud project.


The software is installed from the Centos *extras* repository and from the Calico repositories for Ubuntu.

Role Variables
--------------

    indigo_dc_etcd_bm_peers_list: ["hosh1","host2","host3"]
    indigo_dc_etcd_bm_peers_group: groupname

The two variables above are mutually exclusive. If *indigo_dc_etcd_bm_peers_list* is defined, *indigo_dc_etcd_bm_peers_group* is ignored. 
If *indigo_dc_etcd_bm_peers_list* is not defined, the list of hosts is taken from the inventory group specified in the *indigo_dc_etcd_bm_peers_group* variable. 
The hosts making up the etcd cluster can be specified either in terms of hostname or IP. 
Each host in the inventory is configured using the hostname or the IP defined in the list (group). If none of the list entries corresponds to the host hostname or one of its IPs, the role will fail.

    indigo_dc_etcd_bm_client_port: 2379 (default)
    indigo_dc_etcd_bm_peer_port: 2380 (default)
    indigo_dc_etcd_bm_url_scheme: http (default)

The three variables above are self explanatory. 

    indigo_dc_etcd_bm_initial_cluster_state: "new" (default)
The variable above corresponds to the etcd variable *ETCD_INITIAL_CLUSTER_STATE*, if this option is set to *existing* etcd will attempt to join an existing cluster.

    indigo_dc_etcd_bm_force_new_cluster: true
The corresponding etcd variable  *ETCD_FORCE_NEW_CLUSTER* is considered unsafe, in particular it does not show the expected behaviour in the Ubuntu case. When this variable is set to true, the etcd data dir:

    /var/lib/etcd/default.etcd/member
    
is removed, in order to force the cration of a new cluster. In particular this is needed to create a new one-member cluster. This forces the removal of all the existing members in the cluster and adds the host as unique member. 

Dependencies
------------

There is no dependency on other roles. 

Example Playbook
----------------

Using *indigo_dc_etcd_bm_peers_list* variable:

    - hosts: servers
      remote_user: root
      roles:
        - { role: ansible-role-etcd-bm, indigo_dc_etcd_bm_peers_list: ["host1","host2,"host3"]  }
   
the host name *localost* is allowed for a stndalone cluster.
Using *indigo_dc_etcd_bm_peers_group* variable:

    - hosts: servers
      remote_user: root
      roles:
        - { role: ansible-role-etcd-bm, indigo_dc_etcd_bm_peers_group: servers  }

License
-------

Apache Licence v2 [1]

[1] http://www.apache.org/licenses/LICENSE-2.0

Author Information
------------------

Mailto: svallero AT to.infn.it
