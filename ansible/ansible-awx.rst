.. title:: Ansible AWX 連携

.. _ansible-awx:

-----------------------
Ansible AWX 連携
-----------------------

AWX 連携
+++++++++++++++

0. 前提条件
-----------
- Prism Central 5.11以上
- Calm 2.9以上

1. 概要
-----------

- 動画による解説はこちら: `動画 <https://youtu.be/rWOAB9SLT5U>`_

2. AWXインスタンスの作成
-------------

#. :download:`こちら<./ansible-awx.json>`からブループリントをダウンロードし、Calmへとアップロードします。
    - **Project** - Default
    - **Passphrase** - nutanix/4u
    
#. ブループリントをSaveします。エラー出力が出ますので一度Prism Centralからログアウトし、再度ログインします。

#. ブループリントの内容を確認、修正します。

    - **Credentials** - :ref:`ssh_key_priv`で設定済み。
    - **Application Profile**配下の**Variables**    
        - **public_key** - :ref:`ssh_key_pub`で設定済み。
    - **Service**配下の**AWX**配下の - Nutanixサイトからダウンロードするように設定済み。
        - **Image**
            - **Configuration**配下の**DOWNLOADABLE IMAGE CONFIGURATION**を参照。
        - **Cloud-init** - :ref:`cloudinit`で設定済み。
        - **NETWORK ADAPTERS (NICS)** - 適切なネットワークを選択

#. ブループリントをSaveし、Launchします。

#. AWXアプリケーションの起動に成功したら、ブラウザを開きAWX仮想マシンのIPアドレスにアクセスします。デフォルトの認証情報は**admin/password**です。

    .. figure:: images/awx1.png

3. AWXのセットアップ
------------

#. ナビゲーションペインの**Inventory Scripts**に移動し、緑色の"+"ボタンをクリックし新規スクリプトを作成します。

    .. figure:: images/awx-inv-script.png

    - **NAME** - *prism central*
    - **CUSTOM SCRIPT** - `prism_central.py <https://raw.githubusercontent.com/panlm/ansible-nutanix-prismcentral-inventory/master/prism_central.py>`_をコピー＆ペーストします。
    - **SAVE**をクリックします。

#. ナビゲーションペインの**Inventories**に移動し、緑色の"+"ボタンをクリックし新規インベントリを作成します。

    .. figure:: images/awx-inv1.png

    - **NAME** - *Nutanix Inventory*
    - **SAVE**

#. 上部の**SOURCES**メニューを選択します。

    .. figure:: images/awx-inv2.png

#. 緑色の"+"ボタンをクリックし新たなインベントリのソースを設定します。

    .. figure:: images/awx-inv3.png

    - **NAME** - *prism central*
    - **SOURCE** - *Custom Script*
    - **CUSTOM INVENTORY SCRIPT** - *上記ステップでアップロードしたスクリプトを選択します。*
    - **UPDATE ON LAUNCH** - *有効化
    - **ENVIRONMENT VARIABLES** - *prism_central_ip*, *prism_username*, *prism_password*

        .. code-block:: yaml
        
            ---
            PC_IP_ADDR: 10.42.98.39
            PC_USERNAME: admin
            PC_PASSWORD: nx2Tech264!
    
    - **SAVE**

#. インベントリ同期を開始します。

    - **SOURCE**に移動します。

        .. figure:: images/awx-navigator1.png

    - ``Start sync process``をクリックします。

        .. figure:: images/awx-inv4.png

    - 雲のアイコンが緑になれば同期完了です。

        .. figure:: images/awx-inv5.png

    - 同期完了後、**HOST**タブをクリックすると、Nutanixクラスタの全仮想マシンがインベントリとしてリストされています。

        .. figure:: images/awx-inv6.png

#. ナビゲーションペインから**Credentials**をクリックし、後ほど作成する仮想マシン用の認証情報を登録します。

    .. figure:: images/awx-cred2.png

    - **NAME** - *Nutanix VM*
    - **ORGANIZATION** - *Default*
    - **CREDENTIAL TYPE** - *Machine*
    - **USERNAME** - *centos*
    - **SSH PRIVATE KEY** - こちらのプライベートキーをコピー＆ペースト --> :ref:`ssh_key_priv`
    - **PRIVILEGE ESCALATION METHOD** - *sudo*
    - **PRIVILEGE ESCALATION USERNAME** - *root*
    - **Save**

#. ナビゲーションペインから**Projects**をクリックします。

    - ``Get latest SCM revision``をクリックします。

        .. figure:: images/awx-proj0.png

    - **Demo Project**に移動すると、**SCM URL**を確認できます。こちらはAnsibleのプレイブックのダウンロード元のURLです。

        .. figure:: images/awx-proj2.png

        - **SCM URL** - `https://github.com/panlm/myansible.git`
        - **Save**

    - **JOB TEMPLATES**をクリックします。

        .. figure:: images/awx-proj3.png

    - **Demo Job Template**を開きます。こちらのJobを後続のステップで使用します。

        .. figure:: images/awx-proj4.png

        - **INVENTORY** - *Nutanix Inventory*
        - **CREDENTIAL** - *Nutanix VM*
        - **PLAYBOOK** - *hello_world.yml*

            .. note:: もしプレイブックがみえない場合、``Get latest SCM revision``を再度行います。
            
        - **ALLOW PROVISIONING CALLBACKS** - *有効化*
        - **PROVISIONING CALLBACK URL** - メモします。
        - **HOST CONFIG KEY**のボタンをクリックし、メモします。        
        - **Save**をクリックすると再度Callback URLとHost Config Keyが表示されます。

            .. figure:: images/awx-proj6.png

#. ナビゲーションペインから**Jobs**に移動します。

4. Create VM managed by AWX
---------------------------

#. Download blueprint from HERE: :download:`blueprint: ansible-awx-managed-vm <./ansible-awx-managed-vm.json>`

#. This is a simple blueprint with one service. One bash task in service's **Package** --> **Install** task

    .. note:: here is an sample, use your ``HOST CONFIG KEY`` and ``PROVISIONING CALLBACK URL``
    
    .. code-block:: bash

        set -x
        curl --data "host_config_key=629ff460-d58e-410c-a2d0-5e1557eded27" http://10.42.98.107:80/api/v2/job_templates/5/callback/

#. Modify this blueprint

    - Variables

        - **host_config_key** - *your host config key*
        - **callback_url** - *your callback url*
        - **public_key** - *inject your public key* or refer --> :ref:`ssh_key_pub`

    - Assign a linux image
    - Assign cloudinit script, use your cloudinit script or refer --> :ref:`cloudinit`
    - Add nic and assign network
    - Create a credential 

        - **Credential Name** - *centos*
        - **Username** - *centos*
        - **Secret Type** - *SSH Private Key*
        - **SSH Private Key** - *paste your private key* or refer --> :ref:`ssh_key_priv`

#. Save and launch blueprint

5. Check playbook is running on VM
----------------------------------

#. Back to AWX UI, go to **Jobs**

#. after the VM created by Calm, the job will running automatically to config this VM as your expectation

    .. figure:: images/awx-job1.png

#. click job for more detail information, including the VM ip address

    .. figure:: images/awx-job2.png

Reference
+++++++++

- Jose Gomez's `Github <https://github.com/pipoe2h/ansible-nutanix-prismcentral-inventory>`_ and  `Video <https://youtu.be/rWOAB9SLT5U>`_
- Ansible AWX Project `Github <https://github.com/ansible/awx>`_
