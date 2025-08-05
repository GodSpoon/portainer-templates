# JunimoServer - Stardew Valley Dedicated Server

A 24/7 dedicated server for Stardew Valley multiplayer with web-based administration, automatic backups, and mod support via SMAPI. Supports up to 150 players with persistent world saves and crop protection features.

## Features

- **Always-On Hosting**: Keep your farm running 24/7 without needing to leave the game open
- **Web-Based VNC Access**: Manage your server through a browser interface
- **Automatic Game Download**: Uses Steam to download and manage Stardew Valley
- **SMAPI Integration**: Full support for Stardew Valley mods
- **Persistent Progress**: Crop saver and world persistence when no players are online
- **Multi-Player Support**: Up to 150 concurrent players
- **Health Monitoring**: Built-in container health checks

## Quick Start

### 1. Deploy with Portainer

**Method A: Stack Repository**
1. In Portainer: **Stacks** → **Add stack** → **Repository**
2. Repository URL: `https://github.com/GodSpoon/portainer-templates`
3. Compose path: `stacks/junimoserver/docker-compose.yml`
4. Deploy and configure environment variables

**Method B: Upload Compose File**
1. Download `docker-compose.yml` from this directory
2. In Portainer: **Stacks** → **Add stack** → **Upload**
3. Select the downloaded file

### 2. Configure Environment Variables

Set these **required** environment variables:

| Variable | Description | Required |
|----------|-------------|----------|
| `STEAM_USER` | Steam account username | ✅ Yes |
| `STEAM_PASS` | Steam account password | ✅ Yes |
| `VNC_PASSWORD` | VNC interface password | ✅ Yes |

Optional variables:

| Variable | Default | Description |
|----------|---------|-------------|
| `STEAM_GUARD_CODE` | - | Steam Guard code (if enabled) |
| `GAME_PORT` | 24643 | Game UDP port |
| `VNC_PORT` | 8090 | VNC web interface port |
| `DISABLE_RENDERING` | true | Disable rendering for performance |

### 3. Deploy and Wait

1. Click **Deploy the stack**
2. Wait 5-10 minutes for initial setup and game download
3. Monitor logs: `docker logs sdvd-server -f`

## Access Your Server

| Service | URL/Address | Credentials |
|---------|-------------|-------------|
| **Game Server** | `your-server-ip:24643` | In-game multiplayer |
| **VNC Admin** | `http://your-server-ip:8090` | Your VNC password |

## Post-Deployment Setup

### 1. Server Configuration
1. Access VNC interface at `http://your-server-ip:8090`
2. Use your VNC password to log in
3. Configure server settings through the interface
4. Create or load a farm save

### 2. Firewall Configuration
Open these ports in your firewall:
- **UDP 24643** - Game connections
- **TCP 8090** - VNC interface (secure this appropriately)

### 3. Player Access
Players can connect by:
1. Opening Stardew Valley
2. Click **Co-op** → **Join LAN Game**
3. Enter your server IP: `your-server-ip:24643`

## Adding Mods

JunimoServer supports SMAPI mods for enhanced gameplay:

### 1. Enable Mod Support
Edit your `docker-compose.yml` and uncomment:
```yaml
- ./mods:/data/Stardew/Mods/extra
```

### 2. Add Mod Files
1. Create a `mods` directory next to your compose file
2. Download SMAPI-compatible mods
3. Extract mod folders to the `mods` directory
4. Restart the container

### 3. Popular Mods
- **Generic Mod Config Menu** - In-game mod configuration
- **Automate** - Automated crafting and processing
- **Content Patcher** - Enables content replacement mods
- **TimeSpeed** - Adjustable game time speed

## Backup and Maintenance

### Save Data Backup
Save data is stored in Docker volumes. To backup:

```bash
# Create backup
docker run --rm -v junimoserver_save_data:/data -v $(pwd):/backup alpine \
  tar czf /backup/stardew-saves-$(date +%Y%m%d).tar.gz -C /data .

# Restore backup
docker run --rm -v junimoserver_save_data:/data -v $(pwd):/backup alpine \
  tar xzf /backup/stardew-saves-YYYYMMDD.tar.gz -C /data
```

### Updates
To update the server:
1. Pull latest image: `docker pull sdvd/server:latest`
2. Redeploy the stack in Portainer
3. Monitor logs for successful startup

## Troubleshooting

### Steam Authentication Issues
```bash
# Check logs for Steam errors
docker logs sdvd-server | grep -i steam

# Common solutions:
# 1. Verify Steam credentials are correct
# 2. Add Steam Guard code if required
# 3. Ensure Steam account owns Stardew Valley
```

### Connection Problems
```bash
# Verify ports are accessible
sudo ufw status  # Check firewall
netstat -tlnup | grep -E "(24643|8090)"  # Check port binding

# Common solutions:
# 1. Open UDP port 24643 for game
# 2. Open TCP port 8090 for VNC (secure access)
# 3. Check router port forwarding
```

### Performance Issues
```bash
# Monitor resource usage
docker stats sdvd-server

# Optimization tips:
# 1. Ensure DISABLE_RENDERING=true
# 2. Allocate sufficient RAM (4GB+ recommended)
# 3. Use SSD storage for better I/O
```

### VNC Access Issues
```bash
# Test VNC connectivity
curl -I http://localhost:8090

# Common solutions:
# 1. Verify VNC_PASSWORD is set
# 2. Check port 8090 accessibility
# 3. Try accessing from localhost first
```

## Advanced Configuration

### Custom Game Settings
Server configuration files are located in the save data volume:
- Game settings: World-specific configuration
- Server settings: Player limits, networking options

### Resource Requirements

| Players | RAM | CPU | Storage |
|---------|-----|-----|---------|
| 1-4 | 2GB | 1 core | 2GB |
| 5-10 | 4GB | 2 cores | 3GB |
| 10+ | 6GB+ | 3+ cores | 5GB+ |

### Security Considerations

- **VNC Access**: Restrict to trusted networks or use VPN
- **Strong Passwords**: Use complex VNC passwords
- **Regular Updates**: Keep server image updated
- **Network Isolation**: Consider Docker network isolation
- **Steam Credentials**: Store securely, consider using Steam Guard

## Support Resources

- **GitHub Repository**: https://github.com/stardew-valley-dedicated-server/server
- **Discord Community**: https://discord.gg/w23GVXdSF7
- **Documentation**: https://github.com/stardew-valley-dedicated-server/server/tree/master/docs
- **SMAPI Mods**: https://www.nexusmods.com/stardewvalley

## Project Status

⚠️ **Alpha Release**: JunimoServer is currently in alpha. You may encounter bugs or incomplete features. Check the GitHub repository for known issues and updates.

## License

This template is provided as-is. JunimoServer is open-source under MIT license. Ensure compliance with Stardew Valley's terms of service for multiplayer hosting.
