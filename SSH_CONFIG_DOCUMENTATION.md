# SSH Configuration for Remote Servers

## Server Configuration

### GPC Integration Platform Server
- **Host Alias**: `gpc-platform` or `15.164.117.175`
- **IP Address**: 15.164.117.175
- **Username**: bitnami
- **SSH Key**: GPC-Integration-cert-Platform-20250801.pem
- **Port**: 22

## Cursor AI Remote Connection Setup

### Method 1: Using SSH Config (Recommended)
1. Open Cursor AI
2. Press `Ctrl+Shift+P` to open Command Palette
3. Type and select "Remote-SSH: Connect to Host..."
4. Choose either:
   - `15.164.117.175` (Direct IP)
   - `gpc-platform` (Alias)

### Method 2: Direct Connection
1. Press `Ctrl+Shift+P`
2. Select "Remote-SSH: Connect to Host..."
3. Choose "Add New SSH Host..."
4. Enter: `ssh bitnami@15.164.117.175 -i "C:/Users/samin/.ssh/GPC-Integration-cert-Platform-20250801.pem"`

## Testing Connection
```bash
# Test using alias
ssh gpc-platform

# Test using IP
ssh 15.164.117.175
```

## Configuration Date
- Created: 2025-10-08
- SSH Config Location: `~/.ssh/config`

## Other Configured Servers
1. **IGCAcademy**: igc.gpcacademy.org
2. **IGCAcademyEng**: 3.37.254.118
3. **moodle**: 3.36.246.38

All servers use `bitnami` user with their respective SSH keys.