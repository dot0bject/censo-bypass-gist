# Shadowsocks-libev on Debian (Minimal VM) (Escape the censorship by yourself)

## 1. VM Setup

* Deploy VM.
* GitHub Student Pack → Azure → Create VM → 1core1gb Debian based VM (location should be india for better speeds)
* Open inbound ports:

  * `22/tcp` (SSH)
  * `123/tcp` and `123/udp`

---

## 1.5. Free Port 123 (disable systemd-timesyncd)
Port `123` is used by NTP by default.

```bash
sudo ss -tulnp | grep :123
sudo systemctl stop systemd-timesyncd
sudo systemctl disable systemd-timesyncd
sudo systemctl mask systemd-timesyncd
sudo ss -tulnp | grep :123   # verify it's free
```
---

## 2. Install

```bash
sudo apt update
sudo apt install shadowsocks-libev -y
```

---

## 3. Configure

Edit `/etc/shadowsocks-libev/config.json`:

```json
{
  "server": "0.0.0.0",
  "server_port": 123,
  "password": "StrongPasswordHere",
  "timeout": 300,
  "method": "chacha20-ietf-poly1305",
  "mode": "tcp",
  "fast_open": true
}
```

---

## 4. Enable Service

```bash
sudo systemctl enable shadowsocks-libev
sudo systemctl start shadowsocks-libev
```

Check status:

```bash
sudo systemctl status shadowsocks-libev
```

---

## 5. Firewall (optional if using UFW)

* Firewall Rules are to be modified on VM settings or Networking page of your hosting service, add 123 and 22 in ingress and egress both for forwarding the required ports.
```bash
sudo ufw allow 22/tcp
sudo ufw allow 123/tcp
sudo ufw allow 123/udp
sudo ufw enable
```

---

## 6. Client Setup

* Install Shadowsocks client on device.
* Add server config:

  * **Server**: `<VM_Public_IP>`
  * **Port**: `123`
  * **Password**: `StrongPasswordHere`
  * **Method**: `aes-256-gcm`

---

Windows: download from https://github.com/shadowsocks/shadowsocks-windows/releases

macOS: download from https://github.com/shadowsocks/ShadowsocksX-NG/releases

Linux: use shadowsocks-libev or https://github.com/shadowsocks/shadowsocks-qt5

Android: download from https://play.google.com/store/apps/details?id=com.github.shadowsocks

iOS: v2ray

Add a server profile in the client:

```
Server: <VM_Public_IP>
Port: 123
Password: StrongPasswordHere
Method: chacha20-ietf-poly1305
```
---

That’s it — u know who to contact for further info, regards: 0bject
