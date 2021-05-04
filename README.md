# OpenVPN-client

This is a docker image of an OpenVPN client tied to a SOCKS proxy server.  It is useful to isolate network changes (so the host is not affected by the modified routing).

This supports directory style (where the certificates are not bundled together in one `.ovpn` file) and those that contains `update-resolv-conf`

## Usage

```bash
version: '3'

services:
  openvpn:
    image: ghcr.io/apocalypsor/openvpn-socks
    container_name: openvpn
    tty: true
    volumes:
      - ./openvpn:/etc/openvpn/:ro
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun
    dns:
      - 1.1.1.1
      - 1.0.0.1
      - 8.8.8.8
      - 8.8.4.4
    restart: unless-stopped
```

`./openvpn` should contain *one* OpenVPN `.conf` file. It can reference other certificate files or key files in the same directory.

Then connect to SOCKS proxy through through `localhost:1080` / `local.docker:1080`. For example:

```bash
curl --proxy socks5h://local.docker:1080 ipinfo.io
```
