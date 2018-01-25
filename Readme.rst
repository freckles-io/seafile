Description
***********

This is a repository containing the adapter and required roles to setup a  `Seafile <https://seafile.com>`_ server, either from scratch or using existing data.

To find out how to install *freckles* or use `inaugurate <https://github.com/makkus/inaugurate>`_ to transparently install both *freckles* and *Seafile* in one go, check out: `freckles bootstrap / install <https://docs.freckles.io/en/latest/bootstrap.html>`_


Supported
*********

Currently only tested on Debian Stretch, might well work on other Debian-based distributions. Supporting others would be easy as well, but I don't have the time. Pull requests welcome.

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

    bash <(curl https://freckles.io) freckelize -pw true -r frkl:seafile -f blueprint:seafile_sqlite -t /var/lib/freckles

    # or, with 'freckles' already installed:

    freckelize -r frkl:seafile -f blueprint:seafile_sqlite -t /var/lib/freckles


This will ask you a few details about your setup, which you can all keep at their defaults if you want.
To check whether Seafile was installed successfully, visit 'http://127.0.0.1' in a browser, and use the username 'admin@localhost.home' (if you didn't change it) and password 'change_me'.


Now change the password from 'change_me' to something proper!

More involved setups
====================

I've written a blog post about how to use this adapter for more involved setups. Check it out `here <https://freckles.io/blog/example-seafile>`_.

Backup
******

All the important data will live under ``/var/lib/freckles/seafile_mysql`` which you can easily backup.

If you wanted to restore a vanilla server using such a backup, all you needed to do was put the data on the server (preferably to the same path), and run:

.. code-block:: console

   freckelize -r frkl:seafile -f /var/lib/freckles/seafile_mysql -v /var/lib/freckles/seafile_mysql/seafile.yml
