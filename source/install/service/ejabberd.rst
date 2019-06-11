eJabberD
========

eJabberD is a Rock Solid, Massively Scalable, Infinitely Extensible Realtime Server.

More information can be found here https://www.ejabberd.im/

.. note::

    You will need to activate the Python Virtual Environment.

        ``source ~gwhcp/python3/bin/activate``

    The path to `manage.py` is located in `worker/gwhcp_worker/`.

Installation
------------

* Prepare eJabberD server

    ``python manage.py ejabberd_server_install <domain> <ipaddress>``

* Enable / Start eJabberD

    ``systemctl enable ejabberd.service && systemctl start ejabberd.service``