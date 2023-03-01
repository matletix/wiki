# Installation

Clone me and my submodules (themes) with :
```bash
git clone --recurse-submodules https://github.com/matletix/wiki.git
```

Install hugo : grab the last `.deb` package from the [release page](https://github.com/gohugoio/hugo/releases)

**Warning : get the `extended` version ! Not the normal one**

Example :
```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.110.0/hugo_extended_0.110.0_linux-amd64.deb
sudo apt install ./hugo_extended_0.110.0_linux-amd64.deb
```

Build the static assets with hugo :
```bash
cd wiki/
hugo
```

You're ready to serve your website with Nginx : 
```
server_tokens off;
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        server_name wiki.mathieurollet.com;
        root /var/www/html/wiki/public/;
        index index.html;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

To active SSL with certbot do as *root*:
```bash
apt update
apt install snapd
snap install core
snap refresh core
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
certbot --nginx
```

Test automatic renewal with :
```bash
certbot renew --dry-run
```
The command to renew certbot is installed in one of the following locations:
 - `/etc/crontab/`
 - `/etc/cron.*/*`
 - `systemctl list-timers`

To update the website, modify the content/posts (maybe with `git pull`) and run
`hugo` again to rebuild the assets. To avoid "mixed content" error, don't
forget to update the `baseURL` variable in config.toml to add the `https`
protocol.
