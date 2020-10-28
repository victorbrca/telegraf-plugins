
Installing Telegraf collector agent on FreeNAS.
---

a. Start by creating a folder for Telegraf on one of your pools. I have mine under `/mnt/Volume1/home/system/telegraf`

b. Download the Telegraf tar for FreeBSD ([here](https://github.com/influxdata/telegraf/releases)), extract it and copy the binary and the `.conf` files to the folder we created in the previous step

+ `./telegraf-[version]/usr/bin/telegraf`
+ `./telegraf-[version]/etc/telegraf/telegraf.conf`

c. Create `telegraf.init` in the same folder (make sure to modify the 2 lines with your path)

```none
26 : ${telegraf_conf:="/mnt/Volume1/home/system/telegraf/${name}.conf"}

32 command_args="-crP ${pidfile} /mnt/Volume1/home/system/telegraf/${name} ${telegraf_flags} -config=${telegraf_conf} >> /var/log/telegraf.log 2>&1"
```

```
#!/bin/sh
# $FreeBSD$

# PROVIDE: telegraf
# REQUIRE: DAEMON NETWORKING
# BEFORE: LOGIN
# KEYWORD: shutdown

# Add the following lines to /etc/rc.conf to enable telegrafb:
# telegraf_enable="YES"
#
# telegraf_enable (bool): Set to YES to enable telegraf
# Default: NO
# telegraf_conf (str): telegraf configuration file
# Default: ${PREFIX}/etc/telegraf.conf
# telegraf_flags (str): Extra flags passed to telegraf

. /etc/rc.subr

name="telegraf"
rcvar=telegraf_enable
load_rc_config $name

: ${telegraf_enable:="YES"}
: ${telegraf_flags:="-quiet"}
: ${telegraf_conf:="/mnt/Volume1/home/system/telegraf/${name}.conf"}

# daemon
start_precmd=telegraf_prestart
pidfile="/var/run/${name}.pid"
command=/usr/sbin/daemon
command_args="-crP ${pidfile} /mnt/Volume1/home/system/telegraf/${name} ${telegraf_flags} -config=${telegraf_conf} >> /var/log/telegraf.log 2>&1"

telegraf_prestart() {
# Have to empty rc_flags so they don't get passed to daemon(8)
 rc_flags=""
}

run_rc_command "$1"
```

d. Edit the configuration file (`telegraf.conf`) according to your needs

e. Create a link in `/usr/local/etc/rc.d` for telegraf

```none
ln -s /mnt/Volume1/home/system/telegraf/telegraf.init /usr/local/etc/rc.d/telegraf
```

f. Start the service

```none
service telegraf start
```

g. Check the output of the logs to make sure it's working

```none
# tail /var/log/telegraf.log
2020-10-25T17:34:33Z I! Starting Telegraf 1.16.0
```

h. Create an "Init/Shutdown Script" (change to reflect your path)

```none
ln -s /mnt/Volume1/home/system/telegraf/telegraf.init /usr/local/etc/rc.d/telegraf ; service telegraf start
```

![](https://blog.victormendonca.com/img/how-to-install-telegraf-on-freenas/init.png)

- - -

**Credits:**

+ Victor's Blog: [How to Install Telegraf on FreeNAS](https://blog.victormendonca.com/2020/10/28/how-to-install-telegraf-on-freenas/)
+ Reddit: [Post](https://www.reddit.com/r/freenas/comments/81t2bw/can_i_install_telegraf_on_my_freenas_host/) / [User](https://www.reddit.com/user/nDQ9UeOr/)
