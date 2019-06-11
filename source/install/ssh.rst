SSH Keygen and Installation
===========================

Keys are an alternative way of logging into each machine from a central location.

SSH Keygen
----------

Login as *root* and run the following command.

    ``ssh-keygen -b 4096``

SSH Key Installation
--------------------

Copy the keys to the remote host.

    ``scp id_rsa.pub root@remotehostname:.``

If an *.ssh* folder does not exist see `SSH Keygen`_.

Install the remote key into the */root/.ssh/authorized_keys* file and set permissions.

    | ``cat id_rsa.pub >> /root/.ssh/authorized_keys``
    | ``chmod 600 /root/.ssh/authorized_keys``

Disable Passwords
-----------------

    ``nano /etc/ssh/sshd_config``

    Under *Authentication* add or change these lines:

    .. code-block:: none

        PermitRootLogin prohibit-password
        PubkeyAuthentication yes
        AuthorizedKeysFile      .ssh/authorized_keys

Enable Passwords for certain hosts
----------------------------------

If you must use passwords, we recommend adding the following example at the end of the file.

    ``nano /etc/ssh/sshd_config``

    .. code-block:: none

        Match address 10.2.1.0/24
            PermitRootLogin no
            PasswordAuthentication yes