# Docker in Action

## Heading

Becoming a super hero is a fairly straight forward process:

```
$ give me super-powers
```

{% hint style="info" %}
 Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}

## 4. Working with storage and volumes

4.2 Bind mounts

Bind mounts are mount points used to remount parts of a filesystem tree onto other location. When working with containers, bind mounts attach a user-specified location on the host filesystem to a specific point in a container file tree.

{% hint style="info" %}
How to print all mounts points? if I mount point into container can I see this mount point from host?

[https://unix.stackexchange.com/questions/24182/how-to-get-the-complete-and-exact-list-of-mounted-filesystems-in-linux](https://unix.stackexchange.com/questions/24182/how-to-get-the-complete-and-exact-list-of-mounted-filesystems-in-linux)
{% endhint %}

```bash
touch ~/example.log
cat >~/example.conf <<EOF
server {
  listen 80;
  server_name localhost;
  access_log /var/log/nginx/custom.host.access.log main;
  location / {
  root /usr/share/nginx/html;
  index index.html index.htm;
  }
}
EOF
```

```bash
CONF_SRC=~/example.conf; \
CONF_DST=/etc/nginx/conf.d/default.conf; \
LOG_SRC=~/example.log; \
LOG_DST=/var/log/nginx/custom.host.access.log; \
docker run -d --name diaweb \
  --mount type=bind,src=${CONF_SRC},dst=${CONF_DST} \
  --mount type=bind,src=${LOG_SRC},dst=${LOG_DST} \
  -p 80:80 \
  nginx:latest
```

Expanding on this use case, suppose you want to make sure that the NGINX web server can't change the contents of the configuration volume. Linux provides a mechanism to make mount points read-only.

```bash
docker rm -f diaweb

CONF_SRC=~/example.conf; \
CONF_DST=/etc/nginx/conf.d/default.conf; \
LOG_SRC=~/example.log; \
LOG_DST=/var/log/nginx/custom.host.access.log; \
docker run -d --name diaweb \
  --mount type=bind,src=${CONF_SRC},dst=${CONF_DST},readonly=true \
  --mount type=bind,src=${LOG_SRC},dst=${LOG_DST} \
  -p 80:80 \
  nginx:latest
```



