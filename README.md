ansible-docker-ssh-tunnel
=========

[![Build Status](https://travis-ci.org/chirkin/ansible-docker-ssh-tunnel.svg?branch=master)](https://travis-ci.org/chirkin/ansible-docker-ssh-tunnel)

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
