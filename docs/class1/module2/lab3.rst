Sending arguments to your playbook
==================================

You need to specify “vars” values automatically, such as via a command line.
Passing variables using ``extra-vars`` will override playbook variables.
Use the ``-e``, or ``--extra-vars`` argument of ``ansible-playbook``


**Create pool member forced offline playbook**

#. Create a playbook ``pmoff.yaml``.

   - Type ``nano playbooks/pmoff.yaml``
   - Type the following into the ``playbooks/pmoff.yaml`` file.


   .. code::

    ---

    - name: "Pool member offline"
      hosts: bigips
      gather_facts: False
      connection: local

      vars:
        pools: ""
        pmhost: ""
        pmport: ""
        sstate: "disabled"
        mstate: "disabled"

      environment: "{{ bigip_env }}"

      tasks:
        - name: Modify pool member state
          bigip_pool_member:
            state: present
            session_state: "{{ sstate }}"
            monitor_state: "{{ mstate }}"
            host: "{{ pmhost }}"
            port: "{{ pmport }}"
            pool: "{{ pool }}"

#. Run this playbook enable pool member.

   - Type ``ansible-playbook playbooks/pmoff.yaml -e @creds.yaml --ask-vault-pass -e pool="app1_pl" -e pmhost="10.1.20.12" -e pmport="80"``

   You will be prompted to enter username and password before executing the
   playbook.

#. Verify if pool member 10.1.20.12 is ``forced offline``

   - Select ``Local Traffic -> Pools -> Pool Members -> app1_pl``

.. NOTE::
   This playbook relies on environment variables stored in the ``inventory/group_vars/bigips`` file.  This is where the ``bigip_env`` reference comes in.  Feel free to look at the file in question to get more information on the environment variables.
   ::


**Create pool member enable playbook**

#. Create a playbook ``pmena.yaml``.

   - Type ``nano playbooks/pmena.yaml``
   - Type the following into the ``playbooks/pmena.yaml`` file.


   .. code::

    ---

    - name: "Pool member enable"
      hosts: bigips
      gather_facts: False
      connection: local

      vars:
        pools: ""
        pmhost: ""
        pmport: ""
        sstate: "enabled"
        mstate: "enabled"

      environment: "{{ bigip_env }}"

      tasks:
        - name: Modify pool member state
          bigip_pool_member:
            state: present
            session_state: "{{ sstate }}"
            monitor_state: "{{ mstate }}"
            host: "{{ pmhost }}"
            port: "{{ pmport }}"
            pool: "{{ pool }}"

#. Run this playbook enable pool member.

   - Type ``ansible-playbook playbooks/pmena.yaml -e @creds.yaml --ask-vault-pass -e pool="app1_pl" -e pmhost="10.1.20.12" -e pmport="80"``

   You will be prompted to enter username and password before executing the
   playbook.

#. Verify if pool member 10.1.20.12 is ``enabled``

   - Select ``Local Traffic -> Pools -> Pool Members -> app1_pl``

   .. NOTE::

     This method of specifying values is not reserved for credentials.

     In most cases, it **should not** be used for credentials in fact. This is
     because the Ansible command (including the extra arguments) will show in
     the running process list of your Ansible controller.

     The more common situations are when you are prompting for specific configuration
     related to something on your network. For example, your Playbook may be flexible
     enough to take a given ``region`` or ``cell``.

     Bonus playbook: - modify sstate and mstate to "" and pass the variables via cli to achieve various states.

     ::

      $ ansible-playbook playbooks/pmena.yaml -e @creds.yaml --ask-vault-pass -e pool="app1_pl" -e pmhost="10.1.20.12" -e pmport="80" -e mstate="enabled" -e sstate="disabled"

      The Playbook would not need to change, but you could continually provide values to
      variables in the Playbook to keep from writing them into the actual Playbook itself.
