Description
***********

This is a repository containing the adapter and required roles to setup a  `Seafile <https://seafile.com>`_ server, either from scratch or using existing data.

To find out how to install *freckles* or use `inaugurate <https://github.com/makkus/inaugurate>`_ to transparently install both *freckles* and *Seafile* in one go, check out: `freckles bootstrap / install <https://docs.freckles.io/en/latest/bootstrap.html>`_

Issues / Todos
**************

- doesn't use a sql dump, just /var/lib/mysql for backup, which might be a problem when restoring from a different OS/Mysql version (this could be easily changed though)
- https cert request only works if dns is configured already (nothing that can be done to automate that)
- only tested on Debian Stretch so far (should be easy to make it work for other Distributions/Versions though)

How to use
**********

Simple local install
====================

To quickly setup a new Seafile server on your local machine, using the sqlite backend, the only command you need is:

.. code-block:: console

    curl https://freckles.io | bash -s -- freckelize -r frkl:seafile -f blueprint:seafile_sqlite -t /var/lib/freckles

    # or, with 'freckles' already installed:

    freckelize -r frkl:seafile -f blueprint:seafile_sqlite -t /var/lib/freckles


To check whether Seafile was installed successfully, visit 'http://127.0.0.1' in a browser, and use the username 'admin@localhost.home' and password 'change_me'.

Full install, including mysql and https cert
============================================

This is probably not what you want though, as this service will only listen on the '127.0.0.1' interface, and the *sqlite* backend is not really suitable for production use. To costumize the install on a ready-to-use machine with configured dns, create a text file ``seafile.yml`` and add configuration values like this:

.. code-block:: yaml

    seafile:
      seafile_admin_email: makkus@posteo.de
      seafile_domain: seafile.frkl.io
      request_https_cert: true


(obviously, use your own domain and email address)

Now issue:

.. code-block:: console

    freckelize -r frkl:seafile -f blueprint:seafile_mysql -t /var/lib/freckles -v seafile.yml


This will install and configure both *mysql* and *seafile* (in that order), then it will request a *letsencrypt* https certificate for your domain. Finally it'll install *nginx* and configure it to use the just requested https certificate as well as forward all incoming requests to the appropriate Seafile services (*seafile*, *seahub* and *webdav*).

Backup
******

All the important data will live under ``/var/lib/freckles/seafile_mysql`` which you can easily backup.

If you wanted to restore a vanilla server using such a backup, all you needed to do was put the data on the server, and run:

.. code-block:: console

   freckelize -r frkl:seafile -f /path/to/the/data -v seafile.yml

(ideally you copied the ``seafile.yml`` file into the ``seafile_mysql`` folder before backing up, so you have everything can be backed up together. Haven't really thought about how to make that automatic yet.)
