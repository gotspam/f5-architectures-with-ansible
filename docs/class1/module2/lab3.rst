Sending arguments to your playbook
==================================

You need to specify “vars” values automatically, such as via a command line.
Passing variables using ``extra-vars`` will override playbook variables.
Use the ``-e``, or ``--extra-vars`` argument of ``ansible-playbook``


**Create pool member state playbook**

#. Create a playbook ``pmstate.yaml``.

   - Type ``nano ./playbooks/pmstate.yaml``
   - Type the following into the ``playbooks/pmstate.yaml`` file.


   .. code::

    ---

    - name: "Run a tmsh command"
      hosts: bigips
      gather_facts: False
      connection: local

      vars:
        validate_certs: no
        server: 10.1.1.245
        username: "{{ bigip_user }}"
        password: "{{ bigip_pass }}"
        pools: ""
        pmhost: ""
        pmport: ""
        session_state: "disabled"
        monitor_state: "disabled"

      task:
        - name: Modify pool member state
          bigip_pool_member:
            state: present
            session_state: "{{ session_state }}"
            monitor_state: "{{ monitor_state }}"
            host: "{{ pmhost }}"
            port: "{{ pmport }}"
            pool: "{{ pool }}"

#. Run this playbook.

   - Type ``ansible-playbook playbooks/pmstate.yaml -e @creds.yaml -e @creds.yml --ask-vault-pass -e pool="app1_pl" -e pmhost="10.1.20.12" -e pmport="80"``

   You will be prompted to enter username and password before executing the
   playbook.  If successful, you should see config for virtual servers, pools and nodes.


   .. NOTE::

     Prompting is a great way to get input from the user. It can function in both
     an interactive, and non-interactive way.

     The ``bigip_command`` module is what we recommend for all situations where you
     need to do something that a current module does not support.

     This module will **always** warn you when you use it for things that change
     configuration. These warnings will inform you to file an issue on our Github
     Issue tracker for a feature enhancement.

     Ultimately, the goal we want to get to is to have a suite of modules that
     meets all the needs of customers that use Ansible. Since that is not yet possible,
     the ``bigip_command`` is there to accommodate.

     This module can also be used over SSH, but password SSH is the only method known
     to work at this time.
