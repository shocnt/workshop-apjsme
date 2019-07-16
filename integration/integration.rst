.. title:: LAB: Era, Flow, Epoch Integration Demo

.. _integration:

--------------------------------------
LAB: Era, Flow, Epoch Integration Demo
--------------------------------------

Overview
++++++++

#. Online vide from `HERE <https://www.youtube.com/watch?v=EgqfnI230DI>`_

#. Download video from `HERE <https://www.dropbox.com/s/v1269dwd3jcn6eo/Calm-Epoch-Era-20190405.mp4?dl=0>`_

#. Download blueprint from HERE: :download:`Integration Demo Blueprint <./integration.json>`

Era
+++

.. note:: If you using Era version 1.0.1.5 and above, you need create network manually and named `DEFAULT_OOB_POSTGRESQL_NETWORK`


Deploying Era
-------------

Era is distributed as a virtual appliance that can be installed on either AHV or ESXi. In this lab you will deploy Era to your AHV cluster.

#. Get Era image URL from portal.nutanix.com, and add image to PC with that URL directly. 

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**.

    .. figure:: images_era/2a.png

#. Click **Create VM**.

#. Fill out the following fields:

    - **Name** - *Initials*-Era
    - **Description** - (Optional) Description for your VM.
    - **vCPU(s)** - 4
    - **Number of Cores per vCPU** - 1
    - **Memory** - 16 GiB

    - Select **+ Add New Disk**
        - **Type** - DISK
        - **Operation** - Clone from Image Service
        - **Image** - Era\*.qcow2
        - Select **Add**

    - Select **Add New NIC**
        - **VLAN Name** - Primary
        - Select **Add**

#. Click **Save** to create the VM.

#. Select your Era VM and click **Power On**.

Registering a Cluster
---------------------

#. In **Prism Central > VMs > List**, identify the IP address assigned to your Era VM using the **IP Addresses** column.

#. Open ``https://ERA-VM-IP:8443`` in a new browser tab.

    .. note::

        It may take up to 2 minutes for the Era interface to initialize after booting the VM.

#. Select **I have read and agree to terms and conditions** and click **Continue**.

#. Enter **nutanix/4u** as the **admin** password and click **Set Password**.

#. Login using the following credentials:

    - **Username** - admin
    - **Password** - nutanix/4u

#. On the **Welcome to Era** page, fill in the following information:

    - **Name** - *Your Cluster Name*
    - **Description** - (Optional) Description
    - **Address** - *Your Prism Element Cluster IP*
    - **Prism Element Administrator** - admin
    - **Password** - *Your Prism Element Cluster admin's password*

    .. figure:: images_era/3b2.png

    .. note::

        Era requires a Prism Element account with full administrator access. For ESXi clusters, vCenter must also be registered with Prism Element.

#. Click **Next**.

#. Select the **Default** storage container and click **Next**.

    .. figure:: images_era/3c.png

#. Select the **Primary** VLAN. This is the default network profile that Era will use when provisioning new databases. Do **not** select **Manage IP Address Pool**, as your AHV cluster already has IPAM (DHCP) configured for that network.

    .. figure:: images_era/3d.png

#. Click **Next**.

#. Once Era setup has completed, click **Get Started**.

    .. figure:: images_era/3e2.png


Prepare Prism Central
+++++++++++++++++++++

#. Ensure Flow is enabled. Go to **Prism Central Settings**

    .. figure:: images_integration/enable_flow.png

    .. note::

        You will not find **Enable Flow**, if it was enabled.

#. Some categories will be created manually for this lab.

    - **AppTier** - add ``App`` and ``DB`` if not existed

        .. figure:: images_integration/int2.png

#. Upgrade Calm to newest version (2.6.0.3) with **LCM**

    .. figure:: images_integration/upgrade_calm.png

#. Add ahv cluster to your default project

    - **AHV Cluster** - using your ahv cluster
    - **Network** - using the network with IPAM enabled 

    .. figure:: images_integration/edit_project.png

#. Using default CentOS image, copy this url and **Add Image from URL**

    - ``http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud-1801-01.qcow2``


Customized Blueprint
++++++++++++++++++++

#. Upload blueprint from HERE: :download:`Integration Demo Blueprint <./integration.json>`

#. Customized blueprint to meet your environment

    - In **Default** application profile:

        - **era_ip** - Your Era VM IP address
        - **db_public_key** - Your public key
        - **epoch_aoc_host** - *nutanix.epoch.nutanix.com*
        - **epoch_org_id** - *8cb44812-1cd3-45c4-847d-43f3271d126f*
        - **pc_ip** - Your Prism Central IP address
        - **pc_password** - Your admin's password for Prism Central

    - In **Credentials**:

        - **SSH Private Key** for db_server_creds - Put your private key here
        - **password** for era_creds - **nutanix/4u** (we create this password in previous chapter)
        - **SSH Private Key** for centos - Put your private key here

        .. figure:: images_integration/int1.png
        
    - In **Services**:

        - **app** service

            - **image** - choose the image you just uploaded
            - **network** - assign NIC to VM

#. Launch blueprint

Integration Demo
++++++++++++++++
#. Get application IP address and check application

    - Get IP address from right column

        .. figure:: images_integration/int3.png

    - Open URL: ``http://x.x.x.x:8000/`` to access this online shopping application

        .. figure:: images_integration/int4.png

#. Get postgresql IP address and check database from Era

    - Get IP address from right column

        .. figure:: images_integration/int5.png

    - Get the name of database server in top-right corner, it will be display in Epoch monitoring

        .. figure:: images_integration/int6.png

    - confirm time machine settings has been created for this database.

        .. figure:: images_integration/int7.png

#. Check security policy in Flow was be created automatically.

    .. figure:: images_integration/int8.png

    - Check detail of security policy, we will find database and app in seperate *AppTier*, and this security policy just for *Environment* you just launched

        .. figure:: images_integration/int9.png

#. Check Epoch Monitoring, will see *layer 7* monitoring enabled for postgresql 

    .. figure:: images_integration/int10.png

