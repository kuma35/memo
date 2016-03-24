.. -*- coding: utf-8; mode: rst; -*-

ufw
===

.. code-block:: bash

   $ sudo ufw default DENY
   $ sudo ufw allow OpenSSH
   $ sudo ufw enable

/etc/ufw/applications.d/openssh-outer-server

| [OpenSSH outer]
| title=Secure shell server, an rshd replacement
| description=OpenSSH is a free implementation of the Secure Shell protocol.
| ports=10000/tcp
