uWSGI
=====

The uWSGI project aims at developing a full stack for building hosting services.

More information can be found here https://uwsgi-docs.readthedocs.io/en/latest/

.. note::

    You will need to activate the Python Virtual Environment.

        ``source ~gwhcp/python3/bin/activate``

    The path to `manage.py` is located in `worker/gwhcp_worker/`.

Installation
------------

* Prepare the uWSGI server

    ``python manage.py uwsgi_server_install``

* Enable / Start uWSGI

    ``systemctl enable emperor.uwsgi.service && systemctl start emperor.uwsgi.service``

Configuration
-------------

Each user and domain must have a seperate configuration.

* Create uWSGI configuration

    ``python manage.py uwsgi_create_config <username> <path_to_public_directory>``

Nginx Configuration
-------------------

If you are using the Nginx web server.

* Create Nginx uWSGI Python 3 configuration

    ``python manage.py nginx_create_python3_config <hostname> <username>``