ansible-docker-ssh-tunnel
=========

This role installs and configures the dockerized ssh tunnels.

You need to specify master host, other hosts, processed by playbook will be
configured as client hosts.

Client hosts connects to master host and forwards configured ports from master.

Requirements
------------

This role requires Ansible 2.1.0 or higher, Docker Engine 1.12.1 or higher.
Platform requirements are listed in the metadata file.

Role Variables
--------------

    base_path: '/srv/ssh-tunnel'    # stores auto-generated files like ssh keys
    ssh_tunnel_port: 2222           # communication port
    ssh_tunnel_bind_root: false     # need to be enabled if you want to bind
                                    # system port (in 0—1023 range)
    ssh_tunnel_master: 'master'     # considered the master host

    # an array of local (for client) forwarding settings
    # this will be interpolated in ssh command argument as
    # "-L [bind_address:]port:host:hostport [..]"
    # only "bind_address" is optional
    ssh_tunnel_local_forward: [     
      { "bind_address": "LOCAL_ADDRESS", "port": "LOCAL_PORT",
        "host": "REMOTE_ADDRESS", "hostport": "REMOTE_PORT"
      },
      [..]
    ]

Dependencies
------------

None

Example Playbook
----------------

    - hosts: all
      roles:
        - { role: "ansible-docker-ssh-tunnel",
            ssh_tunnel_master: 'master',
            ssh_tunnel_local_forward: [
              { "bind_address": "127.0.0.1", "port": "8080", "host": "example.com", "hostport": "80" },
            ]
          }

License
-------

BSD

Author Information
------------------


A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
