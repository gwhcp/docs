System User
===========

.. note::

    *gwhcp* is the default user we will use.

``useradd --system -c gwhcp --create-home -d /home/gwhcp -s /bin/nologin --user-group gwhcp``

.. warning::

    Using a different user could introduce serious problems.

* Set permissions

    ``chmod 755 /home/gwhcp``