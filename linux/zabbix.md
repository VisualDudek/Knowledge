# Zabbix

## UserParameter via Zabbix-Agent

edit `/etc/zabbix/zabbix_agentd.conf` 

A user parameter has the following syntax:

`UserParameter=<key>,<command>`

```
#Example of my UserParameter in zabbix_agentd.conf
UserParameter=marcin.sftp,cd /etc/zabbix;./run.me
```

U can test parameter by using Zabbix agent with "-t" flag:

`$ zabbix_agent -t marcin.sftp`

{% hint style="info" %}
if running under root, however, note that the agent may have different permissions when launched as a daemon
{% endhint %}

Restart Zabbix agent: `$ sudo systemctl zabbix-agent`

When U test it from Zabbix Server keep in mind that it will be executed via zabbix user and working dir will be different than `/etc/zabbix` for me it was `/`

[Link Zabbix Doc 5.0 User Parameters](https://www.zabbix.com/documentation/current/manual/config/items/userparameters)

[Link Extending Zabbix agents](https://www.zabbix.com/documentation/current/manual/config/items/userparameters/extending_agent)

### test2

