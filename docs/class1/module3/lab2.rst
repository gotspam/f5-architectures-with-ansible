Declarative - Deploy App with WAF Policy
========================================

You will create a playbook to deploy VS, Pools and associated Members using App Services.

**Create app services using playbook using AS3**

#. Examine playbook ``hackazon.yaml``

   .. NOTE::

     This playbook uses **roles** which contain their own vars, files and tasks. Grouping content by roles allows easy sharing of playbooks with other users.  You may examine the files in roles/hackazon directory.

   - Type ``ls -R roles/hackazon/``
   - Type ``cat playbooks/hackazon.yaml``
   - Type ``cat roles/hackazon/files/hackazon.json``
   - Type ``cat roles/hackazon/tasks/main.yaml``

#. Run this playbook.

   - Type ``ansible-playbook playbooks/hackazon.yaml -e @creds.yaml --ask-vault-pass``

#. Verify results in BIG-IP GUI.
#. From the BIG-IP GUI, select **Local Traffic->Virtual Servers** page.  Note there are no virtual servers listed.  You will need to select ``Hackazon`` partition on the top right to view the ``serviceMain`` service.
#. Select **serviceMain** then **Security->Polocies** and note the ``Hackazon`` created.

#. Run this playbook to teardown.

   - Type ``ansible-playbook playbooks/destroy_all_services.yaml -e @creds.yaml --ask-vault-pass``

#. Verify results in BIG-IP GUI.

.. NOTE::

  Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.
  Additional info on use of roles can be seen at `this link`_.

  .. _this link: https://docs.ansible.com/ansible/2.5/user_guide/playbooks_reuse_roles.html
