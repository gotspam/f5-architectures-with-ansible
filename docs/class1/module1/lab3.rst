Adding pool members to a pool
=============================

**Assign pool members to a pool**

You will select a node which you will assign to a pool ``app1_pl``.  Use the
``bigip_pool_member`` module.

#. Create a playbook ``member.yaml``.

   - Type ``nano ./playbooks/member.yaml``

   - Type the following into the ``playbooks/member.yaml`` file.

     .. image:: /_static/image016.png
         :height: 500px

   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook -i inventory/hosts playbooks/member.yaml``

   If successful, you should see similar results

   .. image:: /_static/image017.png
       :height: 200px

#. Verify BIG-IP results.

   - Select **Local Traffic -> Nodes**

   .. image:: /_static/image018.png
       :height: 180px

#. Add another pool member to pool ``app1_pl``.

   - Type ``nano ./playbooks/member.yaml``
   - Add ``node-2`` as shown in the image below.

   .. image:: /_static/image019.png
       :height: 100px

   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook -i inventory/hosts playbooks/member.yaml``

   If successful, you should see similar results

   .. image:: /_static/image020.png
       :height: 200px

#. Verify BIG-IP results.

   .. NOTE::

    The ``bigip_pool_member`` module can configure pools with the details of
    existing nodes. A node that has been placed in a pool is referred to as
    a “pool member”.

    At a minimum, the ``name`` is required. Additionally, the ``host`` is required
    when creating new pool members.
