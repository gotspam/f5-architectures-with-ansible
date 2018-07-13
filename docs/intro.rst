Getting Started
---------------

The purpose of this guide is to provide a sampling of hands-on exercises to better understand deployment of BIG-IP using Ansible.

Lab Topology
~~~~~~~~~~~~

Each student will have a BIG-IP VE environment with IP addressing as below:

.. image:: /_static/image000.png

Connect to lab
~~~~~~~~~~~~~~

**Accessing the lab environment.**

#. Open a browser and go to http://training.f5agility.com/<instructor_uri>/X (where X is your student number)

#. Look for the xubuntu-jumpbox-vxx.  You will use the xubuntu jumpbox for all the labs. (see below)

.. image:: /_static/image001.png
   :height: 700px

#. You can click on **RDP** to RDP to the Xubuntu jumpbox or you can select the **CONSOLE** link and access the jumpbox via your browser.  **The CONSOLE link requires you turn off pop-up blockers.**

.. image:: /_static/image002.png
   :height: 300px

The Microsoft Remote Desktop Client can be used to access the jumpbox.  If the RDP client does not start automatically after clicking on the **RDP** link, you will need to start it manually.  
If you do not have the RDP client installed, it is available from Microsoft `here <https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients>`_

Going forward, all lab work will be completed through the Jump Host. 

**Connecting to ansible host.**

#. Launch putty and connect to ansible.

#. Change directory to ``ansible/mod1``.

.. list-table::
    :widths: 20 40 40
    :header-rows: 1
    :stub-columns: 1

    * - **Component**
      - **VLAN/IP Address(es)**
      - **Credentials**
    * - Jump Host
      - - **Management:** 10.1.1.51
        - **External:** 10.1.10.51
      - - ``f5student``/``f5DEMOs4u``
    * - Ansible Host
      - - **Management:** 10.1.1.150
        - **External:** 10.1.10.150
        - **Internal:** 10.1.20.150
      - - ``root``/``password``
    * - BIG-IP01
      - - **Management:** 10.1.1.245
        - **External:** 10.1.10.245
        - **Internal:** 10.1.20.245
      - - ``admin``/``admin``
    * - Lamp Host
      - - **Management:** 10.1.1.252
        - **External:** 10.1.10.252
        - **Internal:** 10.1.20.252
      - - ``root``/``default``


