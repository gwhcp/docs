Worker API
==========

The Worker API is the backbone for getting the work done on the server without having to manually do the work yourself.

The Worker API has three different doorways:

#. Celery
#. CLI
#. ReST

Prerequisites
-------------

.. toctree::
   :maxdepth: 1

   PostgreSQL Database </install/service/postgresql>
   GWHCP System User </install/default/user>
   Python Virtual Environment </install/default/python>

Download & Installation
-----------------------

* Change directory to the *gwhcp* user home.

    ``git clone https://github.com/gwhcp/worker.git``

Populate Database
-----------------

* Activate Python Virtual Environment.

    ``source python3/bin/activate``

.. note::

    The path to `manage.py` is located in `worker/gwhcp_worker/`.

* Create Migrations.

    ``python manage.py makemigrations``

* Create Database Structure.

    | ``python manage.py migrate``
    | ``python manage.py migrate jabber --database jabber``

* Deactivate Python Virtual Environment.

    ``deactivate``

SUDO Configuration
------------------

We need to add the *gwhcp* system user to our sudoers configuration.

* Edit */etc/sudoers*.

    .. code-block:: none

       %gwhcp ALL=(ALL) NOPASSWD: ALL


Finalize
--------

Make sure :doc:`MemcacheD </install/service/memcached>` is running.