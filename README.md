# Ansible role to setup PPPoE client.

## What does `usepeerdns` do?

From [Ppp](https://wiki.archlinux.org/index.php/Ppp):

> If usepeerdns option is used, pppd will create the /etc/ppp/resolv.conf file with obtained DNS addresses while
> establishing a connection.

`rp-pppoe` package has hooks function which can run scripts upon pppoe connection and disconnection.

Hook scripts resides in `/etc/ppp/{ip-up.d, ipdown.d}`. Both directory contains a simple `00-dns.sh` script.

If the system has `resolvconf` installed, the script will use it to update DNS config.
Otherwise, it creates backup of `/etc/resolv.conf` and use DNS config from pppoe server.

We can change those two up/down scripts according to our own needs.
Refer to `pppoe_update_resolvconf` option comment in this role's `defaults/main.yml`.

## Running inside LXC container

Please refer to docs in [Xray ansible role](https://github.com/home-router/xray?tab=readme-ov-file#running-inside-lxc-container).

Basically config inventory as follows (assume container running PPPoE is named `router`):

```
# Mark pve1 as PVE host, thus only execute kernel module setup tasks.
[pve_host]
pve1

# Mark router as lxc_container. Thus should not make kernel module related changes.
[lxc_container]
router

[pppoe]
router
pve1
```
