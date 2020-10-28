Network Plugins
===

### Device IP and Status - `telegraf_ip_plugin`

This is a Go plugin that outputs the current network device (excluding loopback), as well as the IP and the status of the interfaces.

The measurement name is called `interface`, with the the network device name as a tag, and the IP address and interface status as fields.

Example output:

```
interface,name=enp0s10 ip_address="192.168.0.10",status=1
interface,name=wg0 ip_address="192.168.1.10",status=1
```

Installation
---

a. Download [main.go](device-ip-status/main.go) to your machine and build it:

```
go build -o telegraf_ip_plugin
```

b. Copy `telegraf_ip_plugin` to `/usr/local/bin/`

```
cp telegraf_ip_plugin /usr/local/bin/.
```

c. Add it to your telegraf config file (usually in `/etc/telegraf/telegraf.conf`)

```
[[inputs.exec]]
   commands = [
     "/usr/local/bin/telegraf_ip_plugin"
   ]
   data_format = "influx"
```

d. Restart telegraf
