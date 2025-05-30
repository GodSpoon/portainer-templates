# qBittorrent VPN Template

A secure qBittorrent deployment with integrated WireGuard VPN support.

## Features
- 🔒 **VPN Kill Switch**: All traffic routed through WireGuard
- 🌐 **No DNS Leaks**: Uses VPN-provided DNS servers
- 📁 **Persistent Storage**: Uses external Docker volumes
- 🔧 **Customizable**: Environment variables for easy configuration
- 🏥 **Health Checks**: Built-in container health monitoring

## Prerequisites
1. WireGuard VPN credentials (private key, preshared key)
2. Existing `torrent` Docker volume
3. Host directories for config and watch folder

## Quick Start
1. Deploy through Portainer using the template
2. Configure environment variables (especially VPN keys)
3. Access web UI at `http://your-server:8080`
4. Default login: `admin` / `adminadmin` (change immediately!)

## Security Notes
- Change default qBittorrent credentials after first login
- Keep WireGuard keys secure
- Container includes automatic kill switch if VPN fails
