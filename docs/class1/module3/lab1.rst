Imperative - Create VS, Pool and Members
=========================================

You will create a consolidated playbook to deploy VS, Pools and associated Members.

**Create consolidated playbook**

#. Create a playbook ``app.yaml``.

   - Type ``nano playbooks/app.yaml``
   - Type the following into the ``playbooks/app.yaml`` file.

   .. code::

    ---

    - name: "Imperative: Create new web app"
      hosts: bigips
      gather_facts: False
      connection: local

      vars:
        vsname: "app2_vs"
        vsip: "10.1.10.20"
        vsport: "443"
        plname: "app2_pl"
        pmport: "80"
        pmhost1: "10.1.20.13"
        pmhost2: "10.1.20.14"
        state: "present"

      environment: "{{ bigip_env }}"

      tasks:
        - name: Adjust virtual server
          bigip_virtual_server:
            name: "{{ vsname }}"
            destination: "{{ vsip }}"
            port: "{{ vsport }}"
            description: "Web App"
            snat: "Automap"
            all_profiles:
              - "tcp-lan-optimized"
              - "clientssl"
              - "http"
              - "analytics"
            state: "{{ state }}"

        - name: Adjust a pool
          bigip_pool:
            name: "{{ plname }}"
            monitors: "/Common/http"
            monitor_type: "and_list"
            slow_ramp_time: "120"
            lb_method: "ratio-member"
            state: "{{ state }}"

        - name: Adjust first node
          bigip_node:
            name: "{{ pmhost1 }}"
            host: "{{ pmhost1}}"
            state: "{{ state }}"

        - name: Adjust second node
          bigip_node:
            name: "{{ pmhost2 }}"
            host: "{{ pmhost2 }}"
            state: "{{ state }}"

        - name: Add nodes to pool
          bigip_pool_member:
            name: "{{ item.name }}"
            host: "{{ item.host }}"
            port: "{{ pmport }}"
            pool: "{{ plname }}"
            state: "{{ state }}"
          with_items:
            - name: "{{ pmhost1 }}"
              host: "{{ pmhost1 }}"
            - name: "{{ pmhost2 }}"
              host: "{{ pmhost2 }}"
          when: state == "present"

        - name: Update a VS
          bigip_virtual_server:
            name: "{{ vsname }}"
            pool: "{{ plname }}"
            state: "{{ state }}"
          when: state == "present"


   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook -e creds.yaml --ask-vault-pass playbooks/app.yaml``

   If successful, you should see similar results

   .. image:: /_static/image011.png
       :height: 180px

#. Run this playbook to teardown app.

   - Type ``ansible-playbook -e creds.yaml --ask-vault-pass playbooks/app.yaml -e state="absent"``
