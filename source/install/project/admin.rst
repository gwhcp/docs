Admin
=====

The Admin Control Panel is for employees only and is where all management is commanded.

Prerequisites
-------------

.. toctree::
   :maxdepth: 1

   Worker <worker>

Configure System User
---------------------

* Create System Users

    ``useradd --system -c vmail --no-create-home -d /var/lib/dovecot -s /bin/nologin --user-group vmail``

Download & Installation
-----------------------

* Change directory to the *gwhcp* user home.

    ``git clone https://github.com/gwhcp/admin.git``

Configure System
----------------

* Activate Python Virtual Environment

    ``source python3/bin/activate``

.. note::

    The path to `manage.py` is located in `admin/gwhcp_admin/`.

* Set hostname

    ``python manage.py system_create_hostname <hostname>``

* Create IP Address

    ``python manage.py system_create_ipaddress <ipaddress> <subnet>``

* Add host

    ``python manage.py system_create_host <hostname> <ipaddress>``

* Prepare the Nginx web server

    ``python manage.py nginx_server_install``

* Prepare the uWSGI server

    ``python manage.py uwsgi_server_install``

* Prepare the Postfix mail server

    ``python manage.py postfix_server_install server``

* Prepare the Dovecot mail server

    ``python manage.py dovecot_server_install``

* Create uWSGI configuration

    ``python manage.py uwsgi_create_config gwhcp /home/gwhcp/admin/gwhcp_admin``

* Create Nginx configuration

    ``python manage.py nginx_create_virtual_config <hostname> <ipaddress> <port> gwhcp``

* Create Nginx logs configuration

    ``python manage.py nginx_create_logs_config <hostname> gwhcp``

* Create Nginx uWSGI Python 3 configuration

    ``python manage.py nginx_create_python3_config <hostname> gwhcp``

* Prepare eJabberD server

    ``python manage.py ejabberd_server_install xmpp.example.com <ipaddress>``

* Enable / Start uWSGI

    ``systemctl enable emperor.uwsgi.service && systemctl start emperor.uwsgi.service``

* Enable / Start Nginx

    ``systemctl enable nginx.service && systemctl start nginx.service``

* Enable / Start Postfix

    ``systemctl enable postfix.service && systemctl start postfix.service``

* Enable / Start Dovecot

    ``systemctl enable dovecot.service && systemctl start dovecot.service``

* Enable / Start eJabberD

    ``systemctl enable ejabberd.service && systemctl start ejabberd.service``

* Prepare Celery daemon

    ``python manage.py daemon_celery_install worker.example.com <ipaddress>``

* Prepare IP Address daemon

    ``python manage.py daemon_ipaddress_install worker.example.com``

Accessing the Admin Control Panel
---------------------------------

The default *admin* user as well as other database related items must first be installed. Run the following command
to start the default process.

    ``python manage.py setup_admin_user <password>``

Replacing *<password>* with the password of your choice.

Before accessing the Admin Control panel for the first time, all static files must first be compiled.

* Collect static files

    ``python manage.py collectstatic``

* Compress static files

    ``python manage.py compress``

* Accessing the control panel for the first time can be achieved by accessing the IP Address you created earlier.

    * For example: http://<ipaddress>/

    * The default username is *admin* and the password is set during the initial installation.