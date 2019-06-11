Dovecot
=======

Dovecot is an open source IMAP and POP3 email server for Linux/UNIX-like systems, written with security primarily in mind.

More information can be found here https://www.dovecot.org/

.. note::

    You will need to activate the Python Virtual Environment.

        ``source ~gwhcp/python3/bin/activate``

    The path to `manage.py` is located in `worker/gwhcp_worker/`.

Installation
------------

* Create System Users

    ``useradd --system -c vmail --no-create-home -d /var/lib/dovecot -s /bin/nologin --user-group vmail``

* Prepare the Dovecot mail server

    ``python manage.py dovecot_server_install``

* Enable / Start Dovecot

    ``systemctl enable dovecot.service && systemctl start dovecot.service``