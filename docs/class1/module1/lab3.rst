Adding pool members to a pool
=============================

**Assign pool members to a pool**

You will select a node which you will assign to a pool ``app1_pl``.  Use the
``bigip_pool_member`` module.

#. Create a playbook ``member.yaml``.

   - Type ``nano playbooks/member.yaml``

   - Type the following into the ``playbooks/member.yaml`` file.

     .. code::

      ---

      - name: Add Pool Members to Pool
        hosts: bigips
        connection: local

        vars:
          valid_certs: no
          username: admin
          password: admin
          server: 10.1.1.245

        tasks:
            - name: Add nodes to pool
              bigip_pool_member:
                description: "app1 web servers"
                host: "{{ item.host }}"
                password: "{{ password }}"
                pool: "app1_pl"
                port: "80"
                server: "{{ server }}"
                user: "{{ username }}"
                validate_certs: "{{ valid_certs }}"
              delegate_to: localhost
              loop:
                - { host: 10.1.20.11 }
                - { host: 10.1.20.12 }

   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook playbooks/member.yaml``

   If successful, you should see similar results

   .. image:: /_static/image017.png
       :height: 200px

#. Verify BIG-IP results.

   - Select **Local Traffic -> Pools -> app1_p1 -> Members**

   .. image:: /_static/image018.png
       :height: 200px

   .. NOTE::

    The ``bigip_pool_member`` module can configure pools with the details of
    existing nodes. A node that has been placed in a pool is referred to as
    a “pool member”.

    At a minimum, the ``name`` is required. Additionally, the ``host`` is required
    when creating new pool members.

    The module has several more options, all of which can be seen at `this link`_.

    .. _this link: https://docs.ansible.com/ansible/latest/modules/bigip_pool_member_module.html
