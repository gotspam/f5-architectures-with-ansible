Declarative - Create VS, Pool and Members
=========================================

You will create a playbook to deploy VS, Pools and associated Members using iApps.

**Create service playbook using iApp**

#. Create a playbook ``iapp.yaml``.

   - Type ``nano playbooks/iapp.yaml``
   - Type the following into the ``playbooks/iapp.yaml`` file.

   .. code::

    ---

    - name: "Declarative: Deploy / teardown web app"
      hosts: bigips
      gather_facts: False
      connection: local

      vars:
        service_name: ""
        service_ip: ""
        service_group: ""
        state: "present"

      environment: "{{ bigip_env }}"

      tasks:
        - import_tasks: getsl.yaml
          when: state == "absent"

        - name: Check if {{ service_name }} is deployed
          meta: end_play
          when: 'state == "absent" and service_name not in (service_list.content|from_json)["items"]'

        - name: Build POST body
          template: src=f5.http.j2 dest=./f5.http.yaml

        - name: Adjust an iApp
          uri:
            url: "https://{{ inventory_hostname }}/mgmt/tm/cloud/services/iapp/{{ service_name }}"
            method: "{{ (state == 'present') | ternary('POST', 'DELETE') }}"
            body: "{{ (lookup('template','f5.http.yaml') | from_yaml) }}"
            body_format: json
            user: "{{ bigip_user }}"
            password: "{{ bigip_pass }}"
            validate_certs: no


   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook playbooks/iapp.yaml -e creds.yaml --ask-vault-pass -e service_name="app4" -e service_ip="10.1.10.40" -e service_group="appservers"``

   If successful, you should see similar results

   .. image:: /_static/image011.png
       :height: 180px

#. Run this playbook to teardown.

   - Type ``ansible-playbook playbooks/iapp.yaml -e creds.yaml --ask-vault-pass -e service_name="app4" -e service_ip="10.1.10.40" -e service_group="appservers" -e state="absent"``