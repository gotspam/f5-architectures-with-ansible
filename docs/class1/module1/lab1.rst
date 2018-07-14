Creating a pool on BIG-IP
=========================

You need to create a pool on a BIG-IP.  Use the ``bigip_pool`` module.

As a reminder, the following assumes that you are using RDP to access the Jump Host provided for this lab, that you are using PuTTY to SSH into the "Ansible" host and that the current working directory in that session is ``/ansible/mod1/``.

**Create a pool on a BIG-IP**

#. Create a playbook ``pool.yaml``.

   - Type ``nano playbooks/pool.yaml``
   - Type the following into the ``playbooks/pool.yaml`` file.


     .. image:: /_static/image010.png
       :height: 300px

   - Ctrl x to save file.

   .. HINT::

      If you encounter syntax errors you can troubleshoot issues and/or the use of an online editor such as http://www.yamllint.com/


#. Run this playbook.

   - Type ``ansible-playbook playbooks/pool.yaml``

   If successful, you should see similar results

   .. image:: /_static/image011.png
       :height: 180px

#. Verify BIG-IP results.

   - Login to BIG-IP Admin GUI with username: ``admin`` and password: ``admin`` using any of the available browsers on the Jump Host. 
   - Select **Local Traffic -> Pools**

   .. image:: /_static/image012.png
       :height: 180px

.. NOTE::

   The ``bigip_pool`` module can configure a number of attributes for a pool.
   At a minimum, the ``name`` is required.

   This module is idempotent. Therefore, you can run it over and over again and
   so-long as no settings have been changed, this module will report no changes.

   Notice how we also included the credentials to log into the device as arguments
   to the task. This is *not* the preferred way to do this, but it illustrates a
   way for beginners to get started without needing to know a less obvious way to
   specify these values.

   The module has several more options, all of which can be seen at `this link`_.
   I have reproduced them below. These are relevant to the 2.5 release of Ansible.

   * ``description``
   * ``lb_method``
   * ``monitor_type``
   * ``monitors``
   * ``name``
   * ``quorum``
   * ``reselect_tries``
   * ``service_down_action``
   * ``slow_ramp_time``

   .. _this link: http://docs.ansible.com/ansible/latest/bigip_pool_module.html
   
   Please note, that, by default, the changes made to the BIG-IP via its REST interface and the Ansible playbooks are not persistent.  In order to save the configuaration changes so that they become persistent, you may add the following snippet to the playbook:

    - name: Save configuration
      bigip_config:
        save: yes
        user: admin
        password: admin
        server: 10.1.1.245
        validate_certs: no

In order to undo the change, a playbook can be created that will remove the pool from the BIP-IP.  This new playbook can look like: 
---

- name: Delete a pool
  hosts: bigips
  connection: local

  tasks:
    - name: Delete app1 server pool
      bigip_pool:
        name: app1_pl
        monitors: "/Common/http"
        lb_method: round-robin
        state: absent
        password: admin
        user: admin
        server: 10.1.1.245
        validate_certs: no

    - name: Save configuration
      bigip_config:
        save: yes
        user: admin
        password: admin
        server: 10.1.1.245
        validate_certs: no
        
