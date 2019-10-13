.. title:: LAB: Terraform Leveraging

.. _terraform:

-------------------------
LAB: Terraform Leveraging
-------------------------

How Easy Terraform Is ！
+++++++++++++++++++++++

Prerequisite
------------

- You should know basic concept of services provided by mainly public cloud. 

    - For AWS, you should know IAM, EC2, etc. 
    - For Alicloud, you should know RAM, ECS, etc.

- You should have an account of public cloud privider (AWS or Alicloud) for this lab with **Programmatic access** enabled.
- You should be a fan of **CLI** (just kidding!)

3-step to deploy AWS EC2 with Terraform
---------------------------------------

#. install **terraform** on your Laptop. (this is Macbook example, for more OS, check link `HERE <https://www.terraform.io/downloads.html>`_)

    - download binary for your OS, for example: :download:`MacOS 64-bit <https://releases.hashicorp.com/terraform/0.12.10/terraform_0.12.10_darwin_amd64.zip>`
    - unzip it
    - move **terraform** to your **/usr/local/bin/**
    - try to run **terraform --version** to verify it is workable

#. create aws config

    - create a new folder for test
    - copy and paste following code, it will help you to create a file named **aws.tf**

        .. code-block:: bash
        
            echo '
            # will create ssh key for each instance, keep key name different
            variable "key_name" {}

            # using your access_key and secret_key
            provider "aws" {
                region = "ap-northeast-2"
                access_key = "xxx"
                secret_key = "xxx"
            }

            # create ssh key pair
            resource "tls_private_key" "example" {
                algorithm = "RSA"
                rsa_bits  = 4096
            }

            # upload public key to aws, using defined name
            resource "aws_key_pair" "generated_key" {
                key_name   = "${var.key_name}"
                public_key = "${tls_private_key.example.public_key_openssh}"
            }

            # save private key to output for CLI
            output "privatekey" {
                value = "${tls_private_key.example.private_key_pem}"
            }            

            # find the image name with regex in your self image list, 
            #data "aws_ami" "image" {
            #    name_regex = "^centos.*"
            #    owners     = ["self"]
            #}

            # or you could get standard image from public
            data "aws_ami" "latest-ubuntu" {
                most_recent = true
                owners = ["099720109477"] # Canonical
                filter {
                    name   = "name"
                    values = ["ubuntu/images/hvm-ssd/ubuntu-xenial-16.04-amd64-server-*"]
                }
                filter {
                    name   = "virtualization-type"
                    values = ["hvm"]
                }
                filter {
                    name   = "root-device-type"
                    values = ["ebs"]
                }
            }

            # create instance
            resource "aws_instance" "example" {
                ami           = data.aws_ami.latest-ubuntu.image_id
                instance_type = "t2.micro"
    
                # using the key name we just create
                key_name      = "${aws_key_pair.generated_key.key_name}"

                tags = {
                    name = "TerrVM"
                }

                # after instance created successfully, will try to create a connection to execute some commands
                connection {
                    host        = "${aws_instance.example.public_ip}"
                    type        = "ssh"
                    user        = "ubuntu" #default user in ami
                    private_key = "${tls_private_key.example.private_key_pem}"
                }

                # remote-exec will execute commands in your AWS EC2 instance
                provisioner "remote-exec" {
                    inline = [
                        "echo ${aws_instance.example.public_ip} > pub_ip_addr",
                    ]
                }

                # local-exec will execute commands in this Terraform VM to save the public ip address to a temproary file
                provisioner "local-exec" {
                    command = "echo ${aws_instance.example.public_ip} > /tmp/pub_ip_addr"
                }
            }

            # print something in stdout
            output "publicip" {
                value = "${aws_instance.example.public_ip}"
            }
            ' |tee aws.tf

#. run **terraform**

    - execute following command to launch aws ec2 instance

        .. code-block:: bash

            terraform init
            terraform apply -var key_name="tfkey-111"

    - execute following command to get some variable we define in output

        .. code-block:: bash

            terraform output publicip
            terraform output privatekey

    - execute following command to terminate aws ec2 instance

        .. code-block:: bash

            terraform init
            terraform destroy -var key_name="tfkey-111"

More explanation about terraform config file
--------------------------------------------

- Comment in code. if you do not understand well, please google it :D
- extend reading

    - `Introduce Terraform <https://blog.gruntwork.io/an-introduction-to-terraform-f17df9c6d180>`_
    - `Using Terraform <https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c>`_

Using Terraform to help Calm support Alicloud
+++++++++++++++++++++++++++++++++++++++++++++

Overview
--------

- Using **Terraform** to focus on IAC (Infrastructure as a Code)
- Using **Calm** to focus on application deployment, and Day 2 operation
- Download blueprints you want to try: 

    - :download:`Alicloud ECS with TF <https://github.com/panlm/NTNX/raw/master/calm/blueprints/Terraform-Alicloud.json>`
    - :download:`AWS EC2 with TF <https://github.com/panlm/NTNX/raw/master/calm/blueprints/Terraform-AWS.json>`

Blueprint
---------

- **Service 1**: New VM

    - New a Service, **Cloud** is **Nutanix**

        .. figure:: images/terr1.png

    - you could just give 1 vcpu / 1 GB memory to this VM
    - execute `terraform` command from this VM, create ECS instance on Alicloud, and get the public ip of the new instance

- **Service 2**: Existed VM

    - New a Service, **Cloud** is **Existing Machine**
    - Get the IP address from previous services `@@{Terraform.alicloud_ecs_pub_ip}@@`

        .. figure:: images/terr2.png

Launch It
---------

- Launch successfully, you will see the public ip of Alicloud ECS

    .. figure:: images/terr3.png



