Store API
=========

The Store API is for all Billing Invoices, Payment Gateways, and Store requests are made.

Prerequisites
-------------

.. toctree::
   :maxdepth: 1

   Worker <worker>

Download & Installation
-----------------------

* Change directory to the *gwhcp* user home.

    ``git clone https://github.com/gwhcp/store.git``

Configure System
----------------

* Activate Python Virtual Environment

    ``source python3/bin/activate``

.. note::

    The path to `manage.py` is located in `store/gwhcp_store/`.

* Set hostname

    ``python manage.py system_create_hostname <hostname>``

* Create IP Address

    ``python manage.py system_create_ipaddress <ipaddress> <port>``

* Add host

    ``python manage.py system_create_host <hostname> <ipaddress>``

* Prepare the Nginx web server

    ``python manage.py nginx_server_install``

* Prepare the uWSGI server

    ``python manage.py uwsgi_server_install``

* Prepare the Postfix mail server

    ``python manage.py postfix_server_install sendmail``

* Create uWSGI configuration

    ``python manage.py uwsgi_create_config gwhcp /home/gwhcp/store/gwhcp_store/``

* Create Nginx configuration

    ``python manage.py nginx_create_virtual_config <hostname> <ipaddress> <port> gwhcp``

* Create Nginx logs configuration

    ``python manage.py nginx_create_logs_config <hostname> gwhcp``

* Create Nginx uWSGI Python 3 configuration

    ``python manage.py nginx_create_python3_config <hostname> gwhcp``

* Enable / Start uWSGI

    ``systemctl enable emperor.uwsgi.service && systemctl start emperor.uwsgi.service``

* Enable / Start Nginx

    ``systemctl enable nginx.service && systemctl start nginx.service``

* Enable / Start Postfix

    ``systemctl enable postfix.service && systemctl start postfix.service``