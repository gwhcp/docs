Postfix
=======

Postfix attempts to be fast, easy to administer, and secure.

More information can be found here http://www.postfix.org

.. note::

    You will need to activate the Python Virtual Environment.

        ``source ~gwhcp/python3/bin/activate``

    The path to `manage.py` is located in `worker/gwhcp_worker/`.

Installation
------------

If you need support for the sendmail functionality only:

    ``python manage.py postfix_server_install sendmail``

If you need support for sending and receiving email:

    ``python manage.py postfix_server_install server``

* Enable / Start Postfix

    .. note::

        Only required when setting up Postfix as a mail server and not sendmail.

    ``systemctl enable postfix.service && systemctl start postfix.service``