.. title:: LAB: Multi Cloud Integration

.. _multicloud:

----------------------------
LAB: Multi Cloud Integration
----------------------------

GCP
+++

#. Login your gcp account under *project* in *nutanix* orgnization, for example **APAC Solution DEMO", create *Service accounts* for your calm demo

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

check this video out

https://www.youtube.com/watch?v=7wNCiw1Jn5A

AWS
+++