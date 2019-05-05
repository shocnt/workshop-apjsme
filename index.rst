.. title:: Labs for APJ SE

.. toctree::
  :maxdepth: 2
  :caption: Released Labs
  :name: _labs
  :hidden:

  cicd/cicd
  integration/integration
  ansible/ansible-awx
  ansible/ansible-tower

.. toctree::
  :maxdepth: 2
  :caption: Working Labs
  :name: _workinglabs
  :hidden:

  multicloud/multicloud
  infoblox/infoblox

.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/glossary
  appendix/otherstuff

.. _getting_started:

-------------
Getting Start
-------------

Welcome to labs for APJ SE


Labs we have
++++++++++++

- Released Labs

    - :ref:`cicd`
    - :ref:`integration`
    - :ref:`ansible-awx`
    - :ref:`ansible-tower`

- Working Labs

    - :ref:`multicloud`
    - :ref:`infoblox`


.. _ssh_key:

Sample SSH Key for all labs
+++++++++++++++++++++++++++

.. _ssh_key_pri:

- Private key

    .. code-block:: language

        -----BEGIN OPENSSH PRIVATE KEY-----
        b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABFwAAAAdzc2gtcn
        NhAAAAAwEAAQAAAQEAsvftfW1XYzdlHrf4wmQ+0vtBkAZb9HjrtrXCqseaf8FoX4k/DQ9F
        N3pJRtVJU020QPtbKgR5MaA9BnS7EldCLK/t0dxZSYJ+7oXdCMyDY2jfTz+yNg+FAEwL1K
        Xtd76obEsTxBGocPT3q7sjwAaKWZ2ZIXKhxPsfxR3gRH9nWqpmf2JeZ5Xo1HeXpePSpEvs
        Cgo38E5qyA2Bm2ALav/bgKXTcbMsvRAoFcrrnuZSM4l51e0eP2T3TgfqWYu0+EnOwcrR1u
        Qv4KNLT6AOoz5DtfWcg1fcs+olOMdh6OxUTj928Vd7GRpkzNoyaznAHd0aLJtrCRGmMUj9
        kwazChBSSwAAA+DCI0mlwiNJpQAAAAdzc2gtcnNhAAABAQCy9+19bVdjN2Uet/jCZD7S+0
        GQBlv0eOu2tcKqx5p/wWhfiT8ND0U3eklG1UlTTbRA+1sqBHkxoD0GdLsSV0Isr+3R3FlJ
        gn7uhd0IzINjaN9PP7I2D4UATAvUpe13vqhsSxPEEahw9PeruyPABopZnZkhcqHE+x/FHe
        BEf2daqmZ/Yl5nlejUd5el49KkS+wKCjfwTmrIDYGbYAtq/9uApdNxsyy9ECgVyuue5lIz
        iXnV7R4/ZPdOB+pZi7T4Sc7BytHW5C/go0tPoA6jPkO19ZyDV9yz6iU4x2Ho7FROP3bxV3
        sZGmTM2jJrOcAd3Rosm2sJEaYxSP2TBrMKEFJLAAAAAwEAAQAAAQAorCuu66CGjdpPRuQj
        2YBllnBp+OgBAVIgbeJVyZMVIbFEtP49S5EhcIsiq+pEIk6qzfUD8YxReOclhnXVTztcyI
        a1wOwRxrRuJMH88+2QNA88BW/M1W4WiTHPG/6BzjScl9tgHds4AJQg1SDkzRe4EhbxAQo+
        qAuqUoXbS1EDy4C96QJIzhI7UiJi139rLxKJSlazl0vUVu9eGxwvVGSh/dP5e2Aqawl7hN
        2evcyQY7aR8Q7POn7bZxN3rEeA8ZXMZjQH1PpRiwrixD83skBhXYIKFL2TJmwaLOKhn3Bm
        DrqMCDIW3JUE5T06B/hsJ6efCsKCkKkNsSz6BU2stOmxAAAAgQCOilL8t3QF90w16HyAIL
        XZrk9JXE+4NtlS4tlzgGZNjOTBTuhMglqNxoivBaRpvaKnVttAC/YEBLqJfGmWhiDOOJK5
        jpxXZPYLrUo6f5JdUFHdTSV3YD2QaeXEyxXicM9n2gX5tuSv+7NnQnltRo3nV6IDgAMnij
        draZ9wVZohxAAAAIEA2a/RVmWH/elLdUKOqCm4KmFbDud4Vb/RJUBFUSHUfW66meKJQ4Z9
        jN4x7deXgCoZX0eEw6TrNH6sEcpNvfOQ1geinwq/yb0II+hStwMmpJwXPPWxqSx91XPyux
        ZHjty3Tyr/jWidjEUhuhYTLgiYUbXrlqhI56NAqtcqEtYzrycAAACBANJ3mhpv2JUOpHTO
        NeEKFvDrh+/nApMzBWXt0Ga1pqQw4kmf81PaFHJ4xN7HJV/nicQst3mj6Bt31oUUw4fksx
        SbSYBrHXPP15R7kIxTPG/otmdl7mW4X/u+NqT9/ar7f7A8RpD+Kp6vE7PZuCVzsu2uXqzJ
        yJQ7SPBlrPxL4Xo9AAAAJnN0ZXZlbnBhbkBzdGV2ZW5wYW5zLU1hY0Jvb2stUHJvLmxvY2
        FsAQIDBA==
        -----END OPENSSH PRIVATE KEY-----

.. _ssh_key_pub:

- Public key

    .. code-block:: language

        ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCy9+19bVdjN2Uet/jCZD7S+0GQBlv0eOu2tcKqx5p/wWhfiT8ND0U3eklG1UlTTbRA+1sqBHkxoD0GdLsSV0Isr+3R3FlJgn7uhd0IzINjaN9PP7I2D4UATAvUpe13vqhsSxPEEahw9PeruyPABopZnZkhcqHE+x/FHeBEf2daqmZ/Yl5nlejUd5el49KkS+wKCjfwTmrIDYGbYAtq/9uApdNxsyy9ECgVyuue5lIziXnV7R4/ZPdOB+pZi7T4Sc7BytHW5C/go0tPoA6jPkO19ZyDV9yz6iU4x2Ho7FROP3bxV3sZGmTM2jJrOcAd3Rosm2sJEaYxSP2TBrMKEFJL stevenpan@stevenpans-MacBook-Pro.local

.. _cloudinit:

- Standarded cloud-init script

    .. code-block:: language

        #cloud-config
        disable_root: False
        ssh_enabled: True
        ssh_pwauth: True
        users:
          - name: centos
            ssh-authorized-keys:
              - ssh-rsa @@{public_key}@@
            sudo: ['ALL=(ALL) NOPASSWD:ALL']


