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

#. `こちら <./ansible-awx.json>`_ からブループリントをダウンロードし、Calmへとアップロードします。
    - **Project** - Default
    - **Passphrase** - nutanix/4u
    
#. 一旦ブループリントをSaveします。エラー出力が出ますので一度Prism Centralからログアウトし、再度ログインします。

#. ブループリントの内容を確認、修正します。
    - **Credentials** - :ref:`ssh_key_priv` で設定済み。
    - **ApplicationProfile**
        - **Variables**    
            - **public_key** - :ref:`ssh_key_pub` で設定済み。
    - **Service**
        - **AWX** 
            - **Image**
                - Nutanixサイトからダウンロードするように設定済み。
                - Configuration配下のDOWNLOADABLE IMAGE CONFIGURATIONを参照。
            - **Cloud-init** - :ref:`cloudinit` で設定済み。
            - **NETWORK ADAPTERS (NICS)** - 適切なネットワークを選択

#. ブループリントをSaveし、Launchします。

#. AWXアプリケーションの起動に成功したら、ブラウザを開きAWX仮想マシンのIPアドレスにアクセスします。デフォルトの認証情報は **admin/password** です。

    .. figure:: images/awx1.png

3. AWXのセットアップ
------------

#. ナビゲーションペインの **Inventory Scripts** に移動し、緑色の"+"ボタンをクリックし新規スクリプトを作成します。

    .. figure:: images/awx-inv-script.png

    - **NAME** - *prism central*
    - **CUSTOM SCRIPT** - `prism_central.py <https://raw.githubusercontent.com/panlm/ansible-nutanix-prismcentral-inventory/master/prism_central.py>`_ をコピー＆ペーストします。
    - **SAVE** をクリックします。

#. ナビゲーションペインの **Inventories** に移動し、緑色の"+"ボタンをクリックし新規インベントリを作成します。

    .. figure:: images/awx-inv1.png

    - **NAME** - *Nutanix Inventory*
    - **SAVE**

#. 上部の **SOURCES** メニューを選択します。

    .. figure:: images/awx-inv2.png

#. 緑色の"+"ボタンをクリックし新たなインベントリのソースを設定します。

    .. figure:: images/awx-inv3.png

    - **NAME** - *prism central*
    - **SOURCE** - *Custom Script*
    - **CUSTOM INVENTORY SCRIPT** - *上記ステップでアップロードしたスクリプトを選択します。*
    - **UPDATE ON LAUNCH** - *有効化*
    - **ENVIRONMENT VARIABLES** - *prism_central_ip*, *prism_username*, *prism_password*

        .. code-block:: yaml
        
            ---
            PC_IP_ADDR: 10.42.98.39
            PC_USERNAME: admin
            PC_PASSWORD: nx2Tech264!
    
    - **SAVE**

#. インベントリ同期を開始します。

    - **SOURCE** に移動します。

        .. figure:: images/awx-navigator1.png

    - ``Start sync process`` をクリックします。

        .. figure:: images/awx-inv4.png

    - 雲のアイコンが緑になれば同期完了です。

        .. figure:: images/awx-inv5.png

    - 同期完了後、 **HOST** タブをクリックすると、Nutanixクラスタの全仮想マシンがインベントリとしてリストされています。

        .. figure:: images/awx-inv6.png

#. ナビゲーションペインから **Credentials** をクリックし、後ほど作成する仮想マシン用の認証情報を登録します。

    .. figure:: images/awx-cred2.png

    - **NAME** - *Nutanix VM*
    - **ORGANIZATION** - *Default*
    - **CREDENTIAL TYPE** - *Machine*
    - **USERNAME** - *centos*
    - **SSH PRIVATE KEY** - こちらのプライベートキーをコピー＆ペースト --> :ref:`ssh_key_priv`
    - **PRIVILEGE ESCALATION METHOD** - *sudo*
    - **PRIVILEGE ESCALATION USERNAME** - *root*
    - **Save**

#. ナビゲーションペインから **Projects** をクリックします。

    - ``Get latest SCM revision``をクリックします。

        .. figure:: images/awx-proj0.png

    - **Demo Project** に移動すると、 **SCM URL** を確認できます。こちらはAnsibleのプレイブックのダウンロード元のURLです。

        .. figure:: images/awx-proj2.png

        - **SCM URL** - `https://github.com/panlm/myansible.git`
        - **Save**

    - **JOB TEMPLATES** をクリックします。

        .. figure:: images/awx-proj3.png

    - **Demo Job Template** を開きます。こちらのJobを後続のステップで使用します。

        .. figure:: images/awx-proj4.png

        - **INVENTORY** - *Nutanix Inventory*
        - **CREDENTIAL** - *Nutanix VM*
        - **PLAYBOOK** - *hello_world.yml*

            .. note:: もしプレイブックがみえない場合、 ``Get latest SCM revision`` を再度行います。
            
        - **ALLOW PROVISIONING CALLBACKS** - *有効化*
        - **PROVISIONING CALLBACK URL** - メモします。
        - **HOST CONFIG KEY** のボタンをクリックし、メモします。        
        - **Save** をクリックすると再度Callback URLとHost Config Keyが表示されます。

            .. figure:: images/awx-proj6.png

#. ナビゲーションペインから **Jobs** に移動します。

4. AWXによって管理される仮想マシンの作成
---------------------------

#. `こちら <./ansible-awx-managed-vm.json>`_ からブループリントをダウンロードし、Calmへとアップロードします。

#. 一旦ブループリントをSaveします。エラー出力が出ますので一度Prism Centralからログアウトし、再度ログインします。

#. ブループリントの内容を確認します。
    - **Package**
        - **Install** タスクでAWXで払い出したCallback URLを呼び出しています。

#. ブループリントを修正します。
    - **ApplicationProfile**
        - **Variables**
            - **host_config_key** - *AWXから取得したコンフィグキー*
            - **callback_url** - *AWXから取得したCallback URL*
            - **public_key** - こちらのパブリックキーをコピー＆ペースト --> :ref:`ssh_key_pub`
    - **Credentials** - :ref:`ssh_key_priv` で設定済み。
    - **Service**
        - **AWXManaged** 
            - **Image**
                - Nutanixサイトからダウンロードするように設定済み。
                - Configuration配下のDOWNLOADABLE IMAGE CONFIGURATIONを参照。
            - **Cloud-init** - :ref:`cloudinit` で設定済み。
            - **NETWORK ADAPTERS (NICS)** - 適切なネットワークを選択

#. ブループリントをSaveし、Launchします。

5. 仮想マシンでプレイブックが実行されているか確認
----------------------------------

#. AWX UIに戻り、ナビゲーションペインから **Jobs** に移動します。

#. 仮想マシンがCalmによって作成されたあと、ジョブがこの仮想マシンで自動的に実行されていることを確認します。

    .. figure:: images/awx-job1.png

#. ジョブをクリックし、仮想マシンのIPアドレス等の詳細情報を確認します。

    .. figure:: images/awx-job2.png

参考
+++++++++

- Jose Gomez's `Github <https://github.com/pipoe2h/ansible-nutanix-prismcentral-inventory>`_ and  `Video <https://youtu.be/rWOAB9SLT5U>`_
- Ansible AWX Project `Github <https://github.com/ansible/awx>`_
