Nginx
=====

Nginx [engine x] is an HTTP and reverse proxy server, a mail proxy server, and a generic TCP/UDP proxy server.

More information can be found here https://nginx.org/

.. note::

    You will need to activate the Python Virtual Environment.

        ``source ~gwhcp/python3/bin/activate``

    The path to `manage.py` is located in `worker/gwhcp_worker/`.

Installation
------------

* Prepare the Nginx web server

    ``python manage.py nginx_server_install``

* Enable / Start Nginx

    ``systemctl enable nginx.service && systemctl start nginx.service``