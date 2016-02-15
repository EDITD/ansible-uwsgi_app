# ansible-uwsgi_app

An ansible role to run (via supervisor and nginx) a uwsgi app.

## But why?

Often there is a repository that sets up python uwsgi apps. However, in order to run the api reliably, we need

* a way to control it and keep it running (supervisor)
* a webserver so that things don't directly access it

To (hopefully!) simplify the app code, this role will simply take a uwsgi app and set up everything needed to run it.

## An example

Imagine a we have a file `/opt/app/my_app.py` that has a [flask](http://flask.pocoo.org) `app`.

I've set this up so that `app_bin_dir` should be your virtualenv bin directory.
If you aren't using virtualenv, I suppose you could use `/usr/local/bin` (or wherever your `python` and `pip` commands are).

```yaml
  - role: EDITD.uwsgi_api
    app_name: my_app
    app_bin_dir: location-of-virtualenv/bin
    app_port: 5000
    app_callable: "my_app:app"
    app_num_processes: 4
    app_user: user-to-run-as
    app_directory: /opt/app
    app_env_vars: {"PRODUCTION_MODE": "true"}
    max_body_size: 1024m  # sets the maximum size of data that can be posted to the nginx server
    restart_app: yes  # optional, ungracefully restarts the app's supervisor task after installing it
    uwsgi_version: 2.0.12  # optional, specify the exact version of uwsgi to be installed
```
