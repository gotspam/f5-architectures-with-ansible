Declarative - Deploy App with WAF Policy using App Services (AS3)
=================================================================

You will create a playbook to deploy VS, Pools and associated Members using App Services.

**Create app services using playbook using AS3**

#. Examine playbook ``playbooks/hackazon_waf.yaml``

   .. NOTE::

     This playbook uses **roles** which contain their own vars, files and tasks. Grouping content by roles allows easy sharing of playbooks with other users.  You may examine the files in roles/hackazon directory.

   - Type ``ls -R roles/hackazon_waf/``
   - Type ``nano playbooks/hackazon_waf.yaml``
   - Type ``nano roles/hackazon_waf/files/hackazon.json``
   - Type ``nano roles/hackazon_waf/tasks/main.yaml``

#. Run this playbook.

   - Type ``ansible-playbook playbooks/hackazon_waf.yaml``

   This playbook will prompt for username and password.  Enter ``admin`` for both.

#. Verify results in BIG-IP GUI and notice WAF policy is associated.

#. Run this playbook to teardown.

   - Type ``ansible-playbook playbooks/destroy_all_services.yaml -e @creds.yaml --ask-vault-pass``

#. Verify results in BIG-IP GUI.

.. NOTE::

  Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.
  Additional info on use of roles can be seen at `this link`_.

  .. _this link: https://docs.ansible.com/ansible/2.5/user_guide/playbooks_reuse_roles.html
