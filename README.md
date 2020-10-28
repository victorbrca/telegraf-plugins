# telegraf-plugins

A List of custom plugins that I use for Telegraf. These plugins output data (or a specific data format) that is not already provided by Telegraf's built-in plugins.


Collector Agents
---

### FreeNAS

Instructions on how to install Telegraf collector agent on FreeNAS.

+ [FreeNAS](./FreeNAS/README.md)

Plug-ins
---

### UPS

Plug-ins that gather data from UPS devices.

+ [UPS](./UPS/README.md)
  - `getUpsData.py` (Python)

### Network

Plug-ins that gather network related data.

+ [Network](./network/README.md)
  - `telegraf_ip_plugin` (Go)

### ZFS

Plug-ins that gather ZFS data.

+ [ZFS](./zfs/README.md)
  - `zfs_pool` for Linux (Bash)
