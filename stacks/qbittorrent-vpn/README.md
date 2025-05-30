# qBittorrent VPN

A secure BitTorrent client with integrated VPN support (OpenVPN/WireGuard), Privoxy HTTP proxy, and microsocks SOCKS5 proxy for enhanced privacy and security.

## Features

- **VPN Integration**: Built-in OpenVPN and WireGuard support
- **IP Leak Protection**: Prevents IP leakage when VPN connection drops
- **Proxy Services**: 
  - Privoxy HTTP proxy for unfiltered web access
  - microsocks SOCKS5 proxy for secure connections
- **Multiple VPN Providers**: Support for PIA, AirVPN, ProtonVPN, and custom configs
- **Web Interface**: Full-featured qBittorrent web UI

## Quick Start

### 1. Deploy with Portainer

**Method A: Custom Template (Git Repository)**
1. In Portainer: Templates → Custom Templates → Add Custom Template
2. Fill in details:
   - **Repository URL**: `https://github.com/GodSpoon/portainer-templates`
   - **Compose Path**: `stacks/qbittorrent-vpn/docker-compose.yml`
3. Deploy and customize environment variables

**Method B: Upload Compose File**
1. Download `docker-compose.yml` from this directory
2. In Portainer: Templates → Custom Templates → Upload
3. Select the downloaded file

### 2. Configure Environment Variables

Copy `.env.example` to `.env` and customize:

```bash
# Essential settings
VPN_ENABLED=yes
VPN_CLIENT=wireguard
LAN_NETWORK=192.168.1.0/24  # Your local network
```

### 3. VPN Setup

#### WireGuard Configuration
1. Deploy the stack
2. Place your WireGuard config file at: `config/wireguard/wg0.conf`
3. Restart the container

**Example WireGuard config structure:**
```ini
[Interface]
PrivateKey = YOUR_PRIVATE_KEY
Address = 100.95.137.132/32
DNS = 10.255.255.3

[Peer]
PublicKey = YOUR_PUBLIC_KEY
AllowedIPs = 0.0.0.0/0, ::/0
Endpoint = your-vpn-server.com:443
PresharedKey = YOUR_PRESHARED_KEY
```

#### OpenVPN Configuration
1. Deploy the stack
2. Extract your provider's OpenVPN files to: `config/openvpn/`
3. Keep only one `.ovpn` file and its certificates
4. Restart the container

## Access Points

| Service | URL | Default Credentials |
|---------|-----|-------------------|
| **qBittorrent WebUI** | `http://your-server:8080` | admin / (check logs) |
| **Privoxy Proxy** | `http://your-server:8118` | No authentication |
| **SOCKS5 Proxy** | `your-server:9118` | admin / socks |

## Configuration Options

### Essential Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `VPN_ENABLED` | `yes` | Enable/disable VPN |
| `VPN_CLIENT` | `wireguard` | VPN client type |
| `VPN_PROV` | `custom` | VPN provider |
| `LAN_NETWORK` | `192.168.1.0/24` | Local network CIDR |

### Volume Mapping

| Host Path | Container Path | Purpose |
|-----------|----------------|---------|
| `./config` | `/config` | Configuration files |
| `torrent` | `/data` | Download storage |
| `/etc/localtime` | `/etc/localtime` | System timezone |

### Port Mapping

| Host Port | Container Port | Service |
|-----------|----------------|---------|
| `8080` | `8080` | qBittorrent WebUI |
| `8118` | `8118` | Privoxy HTTP Proxy |
| `9118` | `9118` | SOCKS5 Proxy |
| `58946` | `58946` | BitTorrent (TCP/UDP) |

## Post-Installation Setup

### 1. Initial Access
- Navigate to `http://your-server:8080`
- Login with username: `admin`
- Find the generated password in: `config/supervisord.log`

### 2. Security Setup
- Change the default qBittorrent password
- Update SOCKS proxy credentials if needed
- Verify VPN connection in container logs

### 3. Download Configuration
- Set download directory to `/data`
- Configure bandwidth limits
- Set up RSS feeds or search plugins

## Troubleshooting

### VPN Connection Issues
```bash
# Check container logs
docker logs qbittorrent-vpn

# Verify VPN config location
# WireGuard: config/wireguard/wg0.conf
# OpenVPN: config/openvpn/*.ovpn
```

### Permission Problems
```bash
# Fix ownership issues
sudo chown -R 1000:1000 ./config
sudo chown -R 1000:1000 /path/to/downloads
```

### Port Conflicts
- Ensure ports aren't used by other services
- Update `.env` file with alternative ports
- Redeploy the stack

## Security Notes

⚠️ **Important Security Considerations**

- Container runs in **privileged mode** for WireGuard support
- Ensure proper network isolation
- Regularly update VPN credentials
- Monitor logs for IP leakage warnings
- Use strong passwords for all services

## VPN Provider Examples

### Private Internet Access (PIA)
```bash
VPN_PROV=pia
VPN_USER=your_username
VPN_PASS=your_password
VPN_CLIENT=openvpn
```

### Custom WireGuard
```bash
VPN_PROV=custom
VPN_CLIENT=wireguard
# Place wg0.conf in config/wireguard/
```

### ProtonVPN
```bash
VPN_PROV=protonvpn
VPN_USER=your_username
VPN_PASS=your_password
VPN_CLIENT=openvpn
```

## Support

- **Container Issues**: Check [binhex/arch-qbittorrentvpn](https://github.com/binhex/arch-qbittorrentvpn) repository
- **qBittorrent**: Official [qBittorrent documentation](https://github.com/qbittorrent/qBittorrent/wiki)
- **VPN Setup**: Consult your VPN provider's documentation

## License

This template is provided as-is for educational and personal use. Ensure compliance with your local laws and VPN provider's terms of service.
