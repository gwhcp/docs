Base Installations
==================

Several operating systems can be used in tandem. You are not forced into using a single OS. This guide only covers
currently supported operating systems. Others may be used however must be used with extreme caution.

Operating Systems
-----------------

.. toctree::
   :hidden:

   Archlinux <os/archlinux/index>

As operating systems and software both evolve quickly over time, we must also evolve.
Especially when it comes to security.

* :doc:`Archlinux <os/archlinux/index>` is our default choice operating system and it uses a rolling release cycle for
  the most up-to-date software.

* FreeBSD is not for the faint of heart. However it is well known for it's hardended structure.

* Ubuntu is a favorable operating system among many Linux enthusiasts.

System User and Python
----------------------

.. toctree::
   :maxdepth: 2

   System User <default/user>
   Python Virtual Environment <default/python>

Configure Projects
------------------

.. toctree::
   :maxdepth: 2

   Worker API <project/worker>
   Admin Control Panel <project/admin>
   Client Control Panel <project/client>
   Store API <project/store>

Configure Services
------------------

.. toctree::
   :maxdepth: 2

   PostgreSQL <service/postgresql>
   uWSGI <service/uwsgi>