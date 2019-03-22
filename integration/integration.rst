.. title:: LAB: Integration Demo

.. _integration:

---------------------
LAB: Integration Demo
---------------------

Era 
+++

Deploying Era
-------------

Era is distributed as a virtual appliance that can be installed on either AHV or ESXi. In this lab you will deploy Era to your AHV cluster.

#. Download Era image from portal.nutanix.com, upload image to PC if you have deployed it. 

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

#. Open \https://*ERA-VM-IP:8443*/ in a new browser tab.

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


Prepare PC
++++++++++
#. add image from following URL: \https://s3.ap-northeast-2.amazonaws.com/panlm-images/centos-template.qcow2*/

#. Enable Flow

    .. figure:: images_integration/enable_flow.png

#. Upgrade Calm to newest version (2.6.0.1) with LCM

#. add ahv cluster to your default project

    .. figure:: images_integration/edit_project.png

#. Please download blueprint :download:`HERE <./integration.json>`

