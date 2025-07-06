# V Rising Dedicated Server Stack

This stack deploys a V Rising dedicated server using the TrueOsiris/vrising Docker image.

## Quick Deploy

1. Copy this stack to your Portainer
2. Modify the environment variables as needed
3. Deploy the stack
4. Wait for initial setup (can take up to 10 minutes)

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `SERVERNAME` | Server name shown in browser | My V Rising Server |
| `WORLDNAME` | World save name | world1 |
| `TZ` | Timezone | UTC |
| `GAMEPORT` | Override game port | (empty - uses 9876) |
| `QUERYPORT` | Override query port | (empty - uses 9877) |
| `GAME_PORT` | Host port mapping for game | 9876 |
| `QUERY_PORT` | Host port mapping for query | 9877 |
| `SERVER_PATH` | Host path for server files | ./vrising/server |
| `DATA_PATH` | Host path for persistent data | ./vrising/data |
| `LOGDAYS` | Days to keep log files | 30 |
| `BRANCH` | Server branch version | (empty - latest) |

## Post-Deployment

1. **Configuration Files**: Located in `{DATA_PATH}/Settings/`
   - `ServerHostSettings.json` - Server settings, passwords, ports
   - `ServerGameSettings.json` - Game mechanics, difficulty, etc.

2. **Port Forwarding**: Forward UDP ports 9876 and 9877 in your router/firewall

3. **Password Protection**: Edit `ServerHostSettings.json` and set a password

4. **RCON**: Can be enabled in `ServerHostSettings.json` for remote administration

## Important Notes

- First startup downloads ~2GB and can take 10+ minutes
- Server files are persistent between container restarts
- When passworded, joining via Steam may not work - use in-game server browser
- Server will be visible in server lists if `ListOnSteam` and `ListOnEOS` are true

## Troubleshooting

- Check container logs for startup progress
- Ensure host directories have proper permissions
- Verify firewall/router port forwarding for external access
- Server config changes require container restart

For more information: https://github.com/TrueOsiris/docker-vrising
