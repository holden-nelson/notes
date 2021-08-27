# Linux Web Server

## Django Web Server Provisioning

These are instructions for setting up a Linux web server that will serve Django applications. Obviously there is some crossover with other stacks. Note that this is provisioning, not deploying - these are things that should only have to be set up, at max, once per project, and that should largely be unaffected by application code

1. If necessary, create the actual server - physical device, VM, droplet, whatever
2. Point domain to droplet
   1. [Set nameservers to DO's](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars)
   2. Create DNS records in DO - A @ and A www, plus any for subdomains
3. SSH and User Accounts
   1. DO will copy your public key into `root`'s home directory's authorized keys file, so you can ssh in with root
   2. Create a sudo user account (I'm partial to the name `dev`)
   3. Use the ssh-copy-id command to copy the key into the new sudo user's account
   4. Change ownership of the `.ssh` directory and it's files to `dev:dev` (or whatever)
   5. On local machine, update `.ssh/config` for simpler connection - `ssh HOSTNAME`

```
Host HOSTNAME
 Hostname DOMAIN NAME
 User dev
 ServerAliveInterval 240
```

4. Install updates
5. Install nginx and start its service
   1. `sudo apt-get install nginx`
   2. `sudo systemctl start nginx`
   3. At this point you should be able to visit the nginx welcome screen at the domain
6. Install `pyenv` and the `pyenv-virtualenv` plugin
   1. Pyenv has [some prerequisites](https://github.com/pyenv/pyenv/wiki#suggested-build-environment ). There are also instructions for verifying correct installation on this page.
   2. Use [pyenv installer](https://github.com/pyenv/pyenv-installer)
   3. Should spit out further instructions

```shell
# (The below instructions are intended for common
# shell setups. See the README for more guidance
# if they don't apply and/or don't work for you.)

# Add pyenv executable to PATH and
# enable shims by adding the following
# to ~/.profile:

export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"

# If your ~/.profile sources ~/.bashrc,
# the lines need to be inserted before the part
# that does that. See the README for another option.

# If you have ~/.bash_profile, make sure that it
# also executes the above lines -- e.g. by
# copying them there or by sourcing ~/.profile

# Load pyenv into the shell by adding
# the following to ~/.bashrc:

eval "$(pyenv init -)"

# Make sure to restart your entire logon session
# for changes to profile files to take effect.

# Load pyenv-virtualenv automatically by adding
# the following to ~/.bashrc:

eval "$(pyenv virtualenv-init -)"
```

7. Set up git
   1. Generate a new key pair on the server
   2. Add that key to github
8. Make directory `~/sites`
9. Make an nginx configuration file in `/etc/nginx/sites-available/DOMAIN` and symlink it to `/etc/nginx/sites-enabled/DOMAIN`

```nginx
server { 
    listen 80;
    server_name DOMAIN www.DOMAIN;

    location /static {
        alias /home/dev/sites/DOMAIN/static;
    }

    location / {
        proxy_set_header Host $host;
        proxy_pass http://unix:/tmp/DOMAIN.socket;
    }
}
```

10. Make a systemd service for gunicorn in `/etc/systemd/gunicorn-DOMAIN.service`
    1. Gunicorn can now start with `sudo systemctl start gunicorn-DOMAIN`. 
    2. Enable it to start on boot `sudo systemctl enable gunicorn-DOMAIN` 

```
[Unit]
Description=Gunicorn server for STAGING_DOMAIN

[Service]
Restart=on-failure
User=dev
WorkingDirectory=/home/dev/sites/STAGING_DOMAIN/source
ExecStart=/home/dev/.pyenv/shims/gunicorn --bind unix:/tmp/STAGING_DOMAIN.socket DJANGO_APP.wsgi:application

[Install]
WantedBy=multi-user.target
```

11. Make an SSL certificate
    1. Install `certbot` and its nginx plugin: `sudo apt install certbot python3-certbot-nginx`
    2. Obtain a certificate: `sudo certbot --nginx -d DOMAIN -d www.DOMAIN`