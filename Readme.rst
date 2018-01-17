This is a repository containing the adapter and required roles to setup a  `Seafile <https://seafile.com>`_ server, either from scratch or using existing data.

To find out how to install *freckles* or use `inaugurate <https://github.com/makkus/inaugurate>`_ to transparently install both *freckles* and *Seafile*, check out: `freckles bootstrap / install <https://docs.freckles.io/en/latest/bootstrap.html>`_

To quickly setup a new Seafile server on your local machine, using the sqlite backend, the only command you need is:

```
curl https://freckles.io | bash -s -- freckelize -r frkl:seafile -f blueprint:seafile_sqlite -t /var/lib/freckles

# or, with *freckles* already installed:

freckelize -r frkl:seafile -f blueprint:seafile_sqlite -t /var/lib/freckles
```

To check whether Seafile was installed successfully, visit 'http://127.0.0.1' in a browser, and use the username 'admin@localhost.home' and password 'change_me'.

This is probably not what you want though, as this service will only listen on the '127.0.0.1' interface, and the *sqlite* backend is not really suitable for production use. To costumize the install on a ready-to-use machine with configured dns, create a text file `seafile.yml` and add configuration values like this:

```
seafile:
  seafile_admin_email: makkus@posteo.de
  seafile_domain: seafile.frkl.io
  request_https_cert: true
```

(obviously, use your own domain and email address)

Now issue:

```
freckelize -r frkl:seafile -f blueprint:seafile_mysql -t /var/lib/freckles -v seafile.yml
```

This will install and configure both *mysql* and *seafile* (in that order), then it will request a *letsencrypt* https certificate for your domain. Finally it'll install *nginx* and configure it to use the just requested https certificate as well as forward all incoming requests to the appropriate Seafile services (*seafile*, *seahub* and *webdav*).
