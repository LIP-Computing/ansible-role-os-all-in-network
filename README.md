Role Name
=========

Openstack network configuration for all-in configuration.
It is designed to configure the public network interfaces of internal instances [2] to be accessible from outside the VM [1].
So that, the role connects internal instances [2] interfaces to the VM interfaces.

Every time that we have new instances linked to floating ips, we need to run the configuration network task.
 
This role provides two kind of network configuration:
* Initial network creation/configuration. (create_networks: True)
  * Create public flat network and its subnet.
  * Create private network associated to the user project (no to admin), and its subnet.
* Configure the interfaces of the internal interfaces (**) to be externally accessible(configure_public_networks: True).

[1] Denominate VM as the Openstack virtual machine or instance in which we install the all-in openstack.
[2] Denominate internal instance as the instance of the openstack installed inside of a Openstack virtual machine(VM).

Requirements
------------

Neutron network properly running in the host by using openvswitch.

In order to allow external access to the internal instances, the public network of the
internal openstack has to satisfy the following requirements: 
* Link the VM to several interfaces, at least 3.
 * One interface to allow access to the VM.
 * One interface to allow to connect a router to the public network.
 * The rest of the interfaces will provide connection to instances. 
* The public subnet pool have to contain ips with the same ips has the VM interfaces.



Role Variables
--------------

The variables are related to the Openstack version, endpoints, IPs, passwords: 

    openstack_version: openstack version ("liberty" by default)
    openstack_controller_host: hostname for controller ( for example "controller")
    openstack_external_net_name: external network name ("public" by default)
    openstack_external_subnet_name: external subnetwork name ("public" by default)
    openstack_external_subnet_gateway: external subnetwork gateway ("192.168.1.254" by default)
    openstack_external_subnet_cird: external subnetwork gateway ("192.168.1.0/24" by default)
    openstack_external_subnet_pool_start: ip to start the external subnet pool("192.168.1.1" by default)
    openstack_external_subnet_pool_end: ip to end the external subnet pool("192.168.1.128" by default)

    openstack_external_interfaces:
      - { interface: eth1, ip: 192.168.1.91/24}
      - { interface: eth2, ip: 192.168.1.92/24}
      - { interface: eth3, ip: 192.168.1.93/24}
    
    openstack_demo_router: demo router name ("router1" by default)
    
    openstack_user_project: user project to create the demo network (demo by default)
    openstack_demo_net_name: private network name ("demo_net" by default)
    openstack_demo_subnet_name: private subnetwork name ("demo_net" by default)
    openstack_demo_subnet_gateway: private subnet gateway ("192.168.200.254" by default)
    openstack_demo_subnet_cidr: private subnet cidr ("192.168.200.0/24" by default)
    openstack_demo_dns_servers: private dns servers ("'8.8.8.8 8.8.8.4'" by default)
    
    
    openstack_admin_user: administration user ("admin" by default)
    openstack_admin_project: administration project name ("admin" by default)
    openstack_admin_password: administration password ("openstack" by default)
    openstack_os_url: authentication url ("http://{{ openstack_controller_host }}:35357/v3" by default)
    openstack_os_identity_api_version: identity api version (3 by default)
    
    admin_auth_env:
      OS_PROJECT_DOMAIN_ID: default
      OS_USER_DOMAIN_ID: default
      OS_PROJECT_NAME: "{{ openstack_admin_project }}"
      OS_USERNAME: "{{ openstack_admin_user }}"
      OS_PASSWORD: "{{ openstack_admin_password }}"
      OS_AUTH_URL: "{{ openstack_os_url }}"
      OS_IDENTITY_API_VERSION: "{{ openstack_os_identity_api_version }}"
    
    # Execution mode
    create_networks: option to create initial networks True|False (True by default)
    configure_public_networks: option to configure public interfaces True|False (True by default)

Playbook Examples
----------------

    - hosts: localhost
      vars:
        openstack_version: "liberty"
        openstack_demo_tenant_id: 1217d4751d91448bac885955cf7cb7d7
        openstack_external_subnet_gateway: 192.168.1.254
        openstack_external_subnet_cird: 192.168.1.0/24
        openstack_external_subnet_pool_start: 192.168.1.1
        openstack_external_subnet_pool_end: 192.168.1.3
        openstack_admin_password: "admin_password"
        create_networks: False
        configure_public_networks: False
        openstack_external_interfaces:
          - { interface: eth1, ip: 192.168.1.1/24}
          - { interface: eth2, ip: 192.168.1.2/24}
          - { interface: eth3, ip: 192.168.1.3/24}
    
      roles:
        - LIP-Computing.os-all-in-network

License
-------

Apache 2.0

Author Information
------------------

Contact: jorgesece@lip.pt
