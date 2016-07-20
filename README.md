debops-contrib.consul
=====================

Bootstrap and configure consul servers and clients. 

Requirements
------------

This role does not install the consul binary itself, so this must be done before. (see debops.hashicorp role to achive this task)

Role Variables
--------------


Dependencies
------------

This role makes use of the following roles:

  - debops.etc_services (for setting service names)
  - debops.ferm 		(firewall and access restrictions to services)
  - debops.nginx 		(webserver infront of the consul web ui (optional))

Example Playbook
----------------


    - hosts: consul_servers
      roles:
         - { role: debops-contrib.consul }

    - hosts: consul_ui
      roles:
         - { role: debops-contrib.consul, consul__ui: true }
         - { role: debops.nginx }


License
-------

BSD

Author Information
------------------

Juergen Waibel <j.waibel@jwd.de>
