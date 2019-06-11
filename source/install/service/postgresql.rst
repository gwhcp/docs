PostgreSQL Database
===================

PostgreSQL is the only supported database. A Master/Slave configuration is widely used and highly recommended however,
you can choose to only use a single Master connection.

We will assume you are already familiar with PostgreSQL.

Default Database Names & Users
------------------------------

There are four databases, each with their own users and passwords.

Database names should **not** be changed, however usernames can be renamed to match your own preferences.

+-----------+----------------+
| Database  | Username       |
+===========+================+
| gwhcp     | gwhcp_user     |
+-----------+----------------+
| jabber    | jabber_user    |
+-----------+----------------+

In addition there is also a single replication user named *replicator*. This user is not attached to any single
database.

Master Configuration
--------------------

* Login to postgres user

    ``su postgres``

* Initialize

    ``initdb -D /var/lib/postgres/data``

* Configure *postgresql.conf*

    ``nano /var/lib/postgres/data/postgresql.conf``

    Change the following settings to match those below::

       listen_addresses = '*'
       wal_level = hot_standby
       max_wal_senders = 5
       wal_keep_segments = 32
       archive_mode = on
       archive_command = 'cp -i %p /var/lib/postgres/archive/%f'

* Create *archive* folder:

    ``mkdir -p /var/lib/postgres/archive``

* Create the *replicator* user.

    ``psql -U postgres``

    .. code-block:: none

       CREATE USER replicator REPLICATION LOGIN ENCRYPTED PASSWORD 'password';

* Configure *pg_hba.conf*

    ``nano /var/lib/postgres/data/pg_hba.conf``

    Add the following settings to match those below::

       local replication replicator           trust
       host  replication replicator 0.0.0.0/0 trust
       host  replication replicator ::1/0     trust

    You may also need additional settings for remote access. Below is an example that could be used::

       host all all 0.0.0.0/0 md5
       host all all ::1/0     md5

* Create Databases & Users

    See `Default Database Names & Users`_ for a detailed table.

    .. note::

        Always replace *database*, *username*, and *password* with the proper details.

    * From the standard CLI

        ``psql -U postgres``

        .. code-block:: none

           CREATE ROLE username NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN ENCRYPTED PASSWORD 'password';
           CREATE DATABASE database;
           REVOKE ALL ON DATABASE database FROM public;
           GRANT CONNECT ON DATABASE database TO username;

    * From the database CLI

        ``psql -U postgres -d database``

        .. code-block:: none

           SET search_path = public;
           ALTER ROLE username IN DATABASE database SET search_path = public;
           GRANT usage, create ON SCHEMA public TO username;

* Restart PostgreSQL so the changes can take effect.

* Create a backup which will later be used on the slave servers.

    ``pg_basebackup -U replicator -D - -P -Ft | bzip2 > backup.tar.bz2``

Slave Configuration
-------------------

* Temporarily move the *postgresql.conf* and *pg_hba.conf* files to another location.

* Remove the contents inside the */var/lib/postgres/data* directory.

* Copy the *backup.tar.bz2* from the Master server to the Slave server inside of the */var/lib/postgres/data* directory.

* Untar *backup.tar.bz2*

    ``tar -xjvf backup.tar.bz2``

* Move the *postgresql.conf* and *pg_hba.conf* configuration files back into the */var/lib/postgres/data* directory.

* Configure *postgresql.conf*

    ``nano /var/lib/postgres/data/postgresql.conf``

    Change the following settings to match those below::

       wal_level = hot_standby
       max_wal_senders = 5
       wal_keep_segments = 32
       hot_standby = on

* Configure *pg_hba.conf*

    ``nano /var/lib/postgres/data/pg_hba.conf``

    Add the following settings to match those below::

       host replication replicator 0.0.0.0/0 trust
       host replication replicator ::1/0     trust

    You may also need additional settings for remote access. Below is an example that could be used::

       host all all 0.0.0.0/0 md5
       host all all ::1/0     md5

* Configure *recovery.conf*

    ``nano /var/lib/postgres/data/recovery.conf``

    .. note::

        Replace *10.1.1.1* with the IP Address of your Master server. The application name *slave1* should also be
        a unique identifier.

    Add the following settings to match those below::

       standby_mode = 'on'
       primary_conninfo = 'host=10.1.1.1 port=5432 user=replicator application_name=slave1'
       restore_command = 'cp -i /var/lib/postgres/archive/%f %p'

* Create *archive* folder:

    ``mkdir -p /var/lib/postgres/archive``

* Restart PostgreSQL so the changes can take effect.

Backup Database
---------------

To create a backup of a certain database.

    ``pg_dump -U username database > database.sql --no-owner``

Restore Database
----------------

To restore a backup to a certain database.

    ``psql -U username -d database -f database.sql``

Upgrade Server
--------------

Upgrading PostgreSQL major versions can be a dangerous operation.

* Stop the server.

* Move the current data to a safe location.

       ``mv /var/lib/postgres/data /var/lib/postgres/olddata``

* Create a new *data* directory.

       ``mkdir /var/lib/postgres/data``

* Set permissions on the new *data* directory.

       ``chown postgres:postgres /var/lib/postgres/data``

* As the *postgres* user, initialize the new data structure.

       | ``su postgres``
       | ``initdb --locale $LANG -E UTF8 -D '/var/lib/postgres/data'``

* Using the *tmp* folder, run the *pg_upgrade* command.

       | ``cd /tmp``
       | ``pg_upgrade -b /opt/pgsql-9.6/bin -B /usr/bin -d /var/lib/postgres/olddata -D /var/lib/postgres/data``

* If you made changes to any configuration files you may need to reconfigure those settings. Once finished,
  restart the server.