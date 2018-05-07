Declarative - Create VS, Pool and Members
=========================================

You will create a consolidated playbook to deploy VS, Pools and associated Members.

**Create consolidated playbook**

#. Create a playbook ``newapp.yaml``.

   - Type ``nano ./playbooks/newapp.yaml``
   - Type the following into the ``playbooks/newapp.yaml`` file.


     .. image:: /_static/image010.png
       :height: 300px

   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook -e creds.yaml playbooks/pool.yaml``

   If successful, you should see similar results

   .. image:: /_static/image011.png
       :height: 180px
