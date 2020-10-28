ZFS
===

This plugin provides Linux with the same output format that is given in FreeBSD. This can be useful when you are using one influxdb instance for all your systems.

Output example:

```
# telegraf_zfs_pool
zfs_pool,health=ONLINE,pool=remote_backup allocated=2136263241216i,capacity=42i,dedupratio=1,fragmentation=0i,free=2845898822144i,size=4982162063360i
```

Installation
---

a. Download [telegraf_zfs_pool](./telegraf_zfs_pool) to `/usr/local/bin/telegraf_zfs_pool`

b. Add the configuration to the `/etc/telegraf/telegraf.conf`

```
[[inputs.exec]]
  commands = [bash "/usr/local/bin/telegraf_zfs_pool"]
  timeout = "5s"
  data_format = "influx"
```

c. Restart telegraf service

```
systemctl restart telegraf.service
```
