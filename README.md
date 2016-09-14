Role Name
=========

Openstack network configuration for all-in configuration. This provides two kind of network configuration:
* Initial network creation/configuration. (create_networks: True)
* Configure the network to use the instance interfaces as public IPs. (It have to executed once the internal instance is running.

The running mode is configured by setting the following
configure_public_networks: True

Requirements
------------

Required neutron network properly running in the host by using openvswitch.

Role Variables
--------------

The variables are related to the Openstack version, endpoints, IPs, passwords: 

    openstack_version: "liberty"
    openstack_controller_host: "controller"
    openstack_external_net_name: external-net
    openstack_external_subnet_name: external-net
    openstack_external_subnet_gateway: 192.168.1.254
    openstack_external_subnet_cird: 192.168.1.0/24
    openstack_external_subnet_pool_start: 192.168.1.91
    openstack_external_subnet_pool_end: 192.168.1.93
    openstack_external_interfaces:
      - { interface: eth1, ip: 192.168.1.91/24}
      - { interface: eth2, ip: 192.168.1.92/24}
      - { interface: eth3, ip: 192.168.1.93/24}
    
    openstack_demo_router: "router1"
    
    openstack_demo_net_name: demo_net
    openstack_demo_subnet_name: demo_subnet
    openstack_demo_tenant_id: xxx
    openstack_demo_subnet_gateway: 192.168.100.254
    openstack_demo_subnet_cidr: 192.168.100.0/24
    openstack_demo_dns_servers: '8.8.8.8 8.8.8.4'
    
    
    openstack_admin_user: "admin"
    openstack_admin_project: "admin"
    openstack_admin_password: "openstack"
    openstack_os_url: "http://{{ openstack_controller_host }}:35357/v3"
    openstack_os_identity_api_version: "3"
    
    admin_auth_env:
      OS_PROJECT_DOMAIN_ID: default
      OS_USER_DOMAIN_ID: default
      OS_PROJECT_NAME: "{{ openstack_admin_project }}"
      OS_USERNAME: "{{ openstack_admin_user }}"
      OS_PASSWORD: "{{ openstack_admin_password }}"
      OS_AUTH_URL: "{{ openstack_os_url }}"
      OS_IDENTITY_API_VERSION: "{{ openstack_os_identity_api_version }}"
    
    # Execution mode
    create_networks: True
    configure_public_networks: True

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
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

Contact: jorgesece@gmail.com
