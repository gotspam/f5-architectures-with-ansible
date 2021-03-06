Imperative - Create VS, Pool and Members using seed file
========================================================

You will create a consolidated playbook to deploy VS, Pools and associated Members.

The definition of the virtual server is now declared in a separate var file and each configuration item is created discretely (imperative model).  Running the playbook will allow you to create a virtual server from end-to-end.

**Create consolidated playbook**

#. Create a playbook ``appseed.yaml``.

   - Type ``nano playbooks/appseed.yaml``
   - Type the following into the ``playbooks/appseed.yaml`` file.

   .. code::

    ---

    - name: "Imperative: Deploy / teardown a web app (VS, pool, nodes)"
      hosts: bigips
      gather_facts: False
      connection: local
      vars_files:
        - ../files/appinfo.yaml

      vars:
        state: "present"

      environment: "{{ bigip_env }}"

      tasks:
        - name: Create virtual server
          bigip_virtual_server:
            name: "{{ vs_name }}"
            destination: "{{ vs_ip }}"
            port: "{{ vs_port }}"
            description: "Web App"
            snat: "{{ vs_snat }}"
            all_profiles:
              - "tcp-lan-optimized"
              - "clientssl"
              - "http"
              - "analytics"
            state: "{{ state }}"

        - name: Create a pool
          bigip_pool:
            name: "{{ pl_name }}"
            monitors: "{{ pl_monitor }}"
            lb_method: "{{ pl_lb }}"
            state: "{{ state }}"

        - name: Create nodes
          bigip_node:
            name: "{{ item.name }}"
            host: "{{ item.host }}"
            state: "{{ state }}"
          loop:
            - { name: "{{ nd_ip1 }}", host: "{{ nd_ip1 }}" }
            - { name: "{{ nd_ip2 }}", host: "{{ nd_ip2 }}" }

        - name: Add nodes to pool
          bigip_pool_member:
            host: "{{ item.host }}"
            port: "{{ nd_port }}"
            pool: "{{ pl_name }}"
            state: "{{ state }}"
          loop:
            - { host: "{{ nd_ip1 }}" }
            - { host: "{{ nd_ip2 }}" }
          when: state == "present"

        - name: Update a VS
          bigip_virtual_server:
            name: "{{ vs_name }}"
            pool: "{{ pl_name }}"
            state: "{{ state }}"
          when: state == "present"

   - Ctrl x to save file.
#. Create appinfo.yaml file

   - Type ``nano files/appinfo.yaml``
   - Type the following into the ``files/appinfo.yaml`` file.

   .. code::

    ---

    pl_name: app3_pl
    pl_monitor: /Common/tcp
    pl_lb: round-robin
    nd_ip1: 10.1.20.15
    nd_ip2: 10.1.20.16
    nd_port: 80
    vs_name: app3_vs
    vs_ip: 10.1.10.30
    vs_port: 443
    vs_snat: automap


#. Run this playbook.

   - Type ``ansible-playbook -e @creds.yaml --ask-vault-pass playbooks/appseed.yaml``

#. Verify results in BIG-IP GUI.

   .. hint::

     You should see app3_vs deployed with 2 pool members.  App should be accessible on https://10.1.10.30.


#. Run this playbook to teardown app.

   - Type ``ansible-playbook -e @creds.yaml --ask-vault-pass playbooks/appseed.yaml -e state="absent"``

#. Verify that app3_vs, pool and nodes should be deleted in BIG-IP GUI.

   .. NOTE::

     This playbook leverages a config seed file in files/appinfo.yaml.  Simply modify this file to deploy a new service.
