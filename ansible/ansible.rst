.. title:: LAB: Ansible Integration

.. _ansible:

------------------------
LAB: Ansible Integration
------------------------

AWX Integration
+++++++++++++++

Create AWX
----------

#. Download default centos image from internet and upload

    - ``http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud-1801-01.qcow2``

#. download blueprint from HERE: :download:`DOWNLOAD <./ansible-awx.json>`

#. After blueprint launched, you will get a workable AWX for following lab

#. Open browser to access the AWX VM's IP address. Credential is **admin/password**

    - ``http://x.x.x.x/`` 

    .. figure:: images/awx1.png

Setup AWX
---------

#. Go to **Inventory Scripts** and add new one

    .. figure:: images/awx-inv-script.png

    - **Name** - *prism central*
    - get script from here ``https://raw.githubusercontent.com/pipoe2h/ansible-nutanix-prismcentral-inventory/master/prism_central.py``
    - **Save**

#. Create a new inventory

    .. figure:: images/awx-inv1.png

    - **Name** - **Nutanix Inventory**
    - **Save**

#. Add a **Source** to this inventory

    .. figure:: images/awx-inv3.png

    - **NAME** - *prism central*
    - **SOURCE** - *Custom Script*
    - **CUSTOM INVENTORY SCRIPT** - *select the inventory script you just added*
    - **UPDATE ON LAUNCH** - *checked*
    - **ENVIRONMENT VARIABLES** - *prism_central_ip*, *prism_username*, *prism_password*

        .. code-block:: yml
        
            ---
            PC_IP_ADDR: 10.42.98.39
            PC_USERNAME: admin
            PC_PASSWORD: nx2Tech264!
    
    - **Save**

#. Start sync

    - Click ``Start sync process`` to capture VMs info to inventory

        .. figure:: images/awx-inv4.png

    - the ``cloud`` icon will tune to green, click it for more detail logs

        .. figure:: images/awx-inv5.png

    - after sync finished, check **HOST** tab. all VMs in your cluster will be captured and display HERE

        .. figure:: images/awx-inv6.png

#. Add credential for VMs which we will create

    - Go to **Credentials** and add new one
    - **NAME** - *Nutanix VM*
    - **CREDENTIAL TYPE** - *Machine*
    - **USERNAME** - *centos*
    - **SSH PRIVATE KEY** - *user your private key*
    - **Save**

    .. figure:: images/awx-cred2.png

#. Go to Projects

    - click ``Get latest SCM revision``, it will download ansible playbooks to local for following lab

        .. figure:: images/awx-proj0.png

    - Open this **Demo Project**, you will find **SCM URL**. this is the github which ansible playbooks download from

        .. figure:: images/awx-proj2.png

    - Click **JOB TEMPLATES**, open **Demo Job Template**, we will re-use it in our lab

        .. figure:: images/awx-proj3.png

    - **INVENTORY** - *Nutanix Inventory* (we just create it)
    - **CREDENTIAL** - *Nutanix VM* (we just create it)
    - **PLAYBOOK** - *hello_world.yml*

        .. note:: if you could not see playbook here, maybe fresh your project (see first step in this paragraph, ``Get latest SCM revision``)
        
    - **ALLOW PROVISIONING CALLBACKS** - *checked*
    - write down the **PROVISIONING CALLBACK URL**, will be used in Calm blueprint
    - click right button to generate **HOST CONFIG KEY**, and write down it, will be used in Calm blueprint
    
        - get help for this **HOST CONFIG KEY**

            .. figure:: images/awx-proj5.png

    .. figure:: images/awx-proj4.png

    - **Save** and you will be prompt these important info

    .. figure:: images/awx-proj6.png

#. Go to **Jobs**, and wait magic happen ...

Create VM managed by AWX
------------------------

#. Download blueprint from HERE: :download:`DOWNLOAD <./ansible-vm.json>`

#. This is a simple blueprint with one service

change variable  ;)

#. Create bash task in service's **Package** --> **Install**

    .. note:: here is an sample, use your ``HOST CONFIG KEY`` and ``PROVISIONING CALLBACK URL``
    
    .. code-block:: bash

        set -x
        curl --data "host_config_key=629ff460-d58e-410c-a2d0-5e1557eded27" http://10.42.98.107:80/api/v2/job_templates/5/callback/

#. Save and launch blueprint

Check playbook is running on VM
-------------------------------

#. Back to AWX UI, go to **Jobs**

#. after the VM created by Calm, the job will running automatically to config this VM as your expectation

    .. figure:: images/awx-job1.png

#. click job for more detail information, including the VM ip address

    .. figure:: images/awx-job2.png



Ansible Tower Integration
+++++++++++++++++++++++++

