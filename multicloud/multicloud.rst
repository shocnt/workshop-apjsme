.. title:: LAB: Multi Cloud Integration

.. _multicloud:

----------------------------
LAB: Multi Cloud Integration
----------------------------

GCP
+++

#. Login your gcp account under *project* in *nutanix* orgnization, for example **APAC Solution DEMO**, create *Service accounts* for your calm demo

    .. figure:: images/gcp1.png

    - Goto **IAM & admin**

    .. figure:: images/gcp1.png

    - Choose **Service accounts** from left panel
    - Choose **Create key** from action list of existed service account in right panel
    - Choose **JSON** format, and then click **CREATE**
    - **Save** the key locally for following demo

#. Update Calm settings

    - In **Settings** of Calm, Goto **Providers**
    - **Add Provider**

        - **Name** - GCP
        - **TYPE** - GCP
        - **Service Account File** - choose the file you just download from gcp
        - **Save** and **Verify**

        .. figure:: images/gcp2.png

#. Update Project settings

    - **Infrastructure** - choose *Local and Cloud resources* if needed
    - **GCP** - choose the GCP account name you just created
    - **Save**

Azure
+++++

#. check this video out: https://www.youtube.com/watch?v=7wNCiw1Jn5A

#. Get directory ID

    - **Azure Active Directory** - **Properities** - **Directory ID**
    - copy directory ID to **Directory/Tenant ID** in azure provider settings

#. Get application ID & key

    - **Azure Active Directory** - **App registrations** - **New application registration**

        - name - *will be used below*
        - app type - Web app / API
        - sign-on URL - https://10.139.80.4:9440/console/#page/explore/calm
        - get **application ID** after click create

        - **settings** in this registered application

            - keys - add description of key and expires
            - save keys, you will get the key value

    - copy application ID to **Application/Client ID** in azure provider settings
    - copy key to **Client Key/Secret** in azure provider settings

#. Get subscription ID

    - **resource groups** - Add
    - open resource group and click **Access control** 
    - **Add** - **Add role assignment**

        - **Role** - **Contributor**
        - **Select** - *the application registration* you just created before
        - **Save**

    - copy subscription ID to **Subscription ID** in azure provider settings

#. now you could save and verify azure provider settings, and add it to your project.

#. Goto **Availability sets**, create new one for your existed resource group.
#. Goto **Network security groups**, create new one for your existed resource group.

    - open the network security group you just create and set **Inbound security rules**
    - open port you want to expose

#. Goto **Virtual Networks**, create on for your internal network

    - **Address space** - 10.255.255.0/24
    - **Address range** - 10.255.255.0/25

