# YAHLP - Yet Another HomeLab Portal - Unraid Templates

Specialized Unraid container templates for **YAHLP** deployment with pre-configured environment variables for different deployment modes and authentication methods.

## Overview

YAHLP (Yet Another HomeLab Portal) is a comprehensive reverse proxy and unified media server dashboard that supports:
- **17 services** (Radarr, Sonarr, Plex, Emby, Jellyfin, and more)
- **4 dashboard styles** (Classic, Modern, Sleek, Minimal)
- **Dark/Light theme toggle** with persistent preference
- **Multiple authentication methods** (None, Basic Auth, Google OAuth2, Entra ID)
- **Public and Private deployment modes**
- **Automatic HTTPS** (Let's Encrypt) for public deployments
- **Subdomain-based routing** for media services

## Available Templates

Choose the template that matches your deployment scenario:

### 1. **Private Mode with Basic Auth**
**File:** `yahlp-private-basic.xml`

**Best for:** Internal-only deployments on a trusted local network

**Features:**
- HTTP only (no SSL needed)
- Basic authentication (username/password)
- Internal IP address access (e.g., `http://192.168.x.x`)
- No certificate management
- Minimal resource usage

**Required Configuration:**
- `IP`: Container IP address on your network
- `BASIC_AUTH_CREDENTIALS`: Username and password (format: `user:pass` or `user1:pass1|user2:pass2`)

**Recommended for:** Home labs, internal networks, testing

---

### 2. **Public Mode with Basic Auth**
**File:** `yahlp-public-basic.xml`

**Best for:** External access with username/password protection

**Features:**
- HTTPS via Let's Encrypt (automatic SSL certificates)
- Basic authentication (username/password)
- Full domain-based access (e.g., `https://yourdomain.com`)
- Automatic certificate renewal
- Can add subdomain routing (e.g., `emby.yourdomain.com`)

**Required Configuration:**
- `DOMAIN`: Your domain name (e.g., `yourdomain.com`)
- `EMAIL`: Email for Let's Encrypt notifications
- `BASIC_AUTH_CREDENTIALS`: Username and password (format: `user:pass` or `user1:pass1|user2:pass2`)
- Port forwarding: 80 and 443 to your Unraid server

**Recommended for:** External access with simple authentication

---

### 3. **Public Mode with Google OAuth**
**File:** `yahlp-public-google.xml`

**Best for:** External access protected by Google account authentication

**Features:**
- HTTPS via Let's Encrypt (automatic SSL certificates)
- Google OAuth2 authentication (requires Google Cloud setup)
- Full domain-based access (e.g., `https://yourdomain.com`)
- Automatic certificate renewal
- Can add subdomain routing for media services

**Required Configuration:**
- `DOMAIN`: Your domain name (e.g., `yourdomain.com`)
- `EMAIL`: Email for Let's Encrypt notifications
- `GOOGLE_CLIENT_ID`: From Google Cloud Console
- `GOOGLE_CLIENT_SECRET`: From Google Cloud Console
- `GOOGLE_REDIRECT_URI`: OAuth redirect URL (usually `https://yourdomain.com` or `https://yourdomain.com/oauth2callback`)
- Port forwarding: 80 and 443 to your Unraid server

**Setup Steps:**
1. Create a project in [Google Cloud Console](https://console.cloud.google.com)
2. Create OAuth 2.0 credentials (Web application)
3. Add authorized redirect URI
4. Copy Client ID and Secret to template

**Recommended for:** External access with Google account-based authentication

---

### 4. **Public Mode with Entra ID (Microsoft) OAuth**
**File:** `yahlp-public-entra.xml`

**Best for:** External access protected by Microsoft/Azure AD authentication

**Features:**
- HTTPS via Let's Encrypt (automatic SSL certificates)
- Entra ID (Azure AD) OAuth2 authentication
- Full domain-based access (e.g., `https://yourdomain.com`)
- Automatic certificate renewal
- Can add subdomain routing for media services
- Enterprise-grade authentication

**Required Configuration:**
- `DOMAIN`: Your domain name (e.g., `yourdomain.com`)
- `EMAIL`: Email for Let's Encrypt notifications
- `ENTRA_CLIENT_ID`: Azure AD application ID
- `ENTRA_CLIENT_SECRET`: Azure AD application secret
- `ENTRA_REDIRECT_URI`: OAuth redirect URL (e.g., `https://yourdomain.com/auth/oauth2/callback`)
- `ENTRA_PROVIDER_METADATA_URL`: OpenID configuration URL from Azure AD
- `ENTRA_CRYPTO_PASSPHRASE`: Session encryption key (auto-generated if left blank)
- Port forwarding: 80 and 443 to your Unraid server

**Setup Steps:**
1. Go to [Azure Portal](https://portal.azure.com)
2. Register a new application in Azure AD
3. Configure OAuth 2.0 redirect URI
4. Create a client secret
5. Get OpenID configuration URL from app settings
6. Copy credentials to template

**Recommended for:** Enterprise networks, organizations using Microsoft 365

---

## Quick Start

### For Private Mode (Basic Auth):
1. Download `homelabportal-private-basic.xml`
2. In Unraid, go to `Docker` → `Add Container`
3. Click `Select a template`
4. Paste the template content or load from file
5. Configure:
   - `IP`: Your container IP (e.g., `192.168.x.x`)
   - `BASIC_AUTH_CREDENTIALS`: `user:password`
   - Enable the services you want to use
   - Set backend service URLs (default values provided)
6. Click `Apply`

### For Public Mode (Any Auth Type):
1. Download the appropriate template for your auth method
2. Ensure you have:
   - A domain name (DNS pointing to your Unraid server)
   - Port forwarding configured (80 → 80, 443 → 443)
   - Appropriate OAuth credentials (if using OAuth)
3. In Unraid, load the template and configure:
   - `DOMAIN`: Your domain
   - `EMAIL`: Your email for Let's Encrypt
   - Authentication credentials for your chosen auth method
   - Enable services and set backend URLs
4. Click `Apply`
5. Wait for Let's Encrypt certificate generation (check logs)

---

## Service Configuration

All templates come with service URL placeholders that you must configure:
- `YOUR_BACKEND_HOST` - Replace with your backend service host (e.g., `192.168.x.x` or internal hostname)
- Standard ports for each service are pre-configured
- Update all service URLs to match your network setup

**Example configuration:**
If your services are running on `192.168.x.x`:
- `SABnzbd URL`: `http://192.168.x.x:8080/sabnzbd`
- `NZBGet URL`: `http://192.168.x.x:6789/nzbget`
- `Radarr URL`: `http://192.168.x.x:7878`
- etc.

Or use hostnames if you have DNS configured:
- `SABnzbd URL`: `http://sabnzbd.local:8080/sabnzbd`
- `NZBGet URL`: `http://nzbget.local:6789/nzbget`
- etc.

### Supported Services (17 total):

**Downloads/USENET:**
- SABnzbd
- NZBGet
- NZBHydra

**Torrents:**
- Deluge
- Transmission
- qBittorrent

**Content Management:**
- Radarr (Movies)
- Sonarr (TV)
- Lidarr (Music)
- Whisparr (Adult)

**Search/Requests:**
- Prowlarr (Indexer Manager)
- Seerr (Request Manager)
- Bazarr (Subtitles)

**Media Streaming:**
- Emby
- Plex
- Jellyfin
- Tautulli (Analytics)

---

## Optional Features

### Subdomain Routing (Public Mode Only)
For better organization, you can route media services to dedicated subdomains:
- `Emby Subdomain`: Set to `emby.yourdomain.com` (requires DNS and HTTPS)
- `Plex Subdomain`: Set to `plex.yourdomain.com` (requires DNS and HTTPS)

These services will then be accessible via their subdomains with the same authentication as the main dashboard.

### Dashboard Customization
All templates support:
- **STYLE**: Dashboard appearance (classic, modern, sleek, minimal)
- **DASHBOARD_NAME**: Custom title for your dashboard
- **DASHBOARD_ICON**: Path to custom icon (default: `/icons/yahlp.png`)
- **DASHBOARD_LANDING**: Default page when accessing dashboard (e.g., `radarr`, `sonarr`)
- **DASHBOARD_THEME**: Dark (default) or light
- **DASHBOARD_ORDER**: Service category display order

---

## Troubleshooting

### Let's Encrypt Certificate Issues (Public Mode)
- Ensure ports 80 and 443 are forwarded correctly
- Check that your domain DNS is pointing to your IP
- Check container logs for certbot errors

### Authentication Not Working
- Verify authentication credentials are set correctly
- For OAuth, ensure redirect URIs match exactly
- Check that the OAuth application is properly configured in your provider's console

### Services Not Responding
- Verify backend service URLs are correct for your network
- Ensure services are running and accessible from the container
- Check firewall rules if services are on a different subnet

### Performance Issues
- In private mode, consider using the lighter dashboard styles (sleek, minimal)
- Disable unused services to reduce proxy overhead
- Check container resource limits in Unraid

---

## Security Notes

### Private Mode
- Basic Auth is suitable for trusted networks only
- Consider using VPN for external access instead of exposing directly
- No encryption by default (HTTP only)

### Public Mode with Basic Auth
- Basic Auth transmits credentials in Base64 (visible if HTTPS is compromised)
- Consider using OAuth2 for better security
- Keep your domain HTTPS certificate updated

### Public Mode with OAuth
- OAuth2 is more secure than Basic Auth
- Choose OAuth provider based on your trust and availability
- Ensure OAuth application secrets are kept confidential
- Use strong, unique crypto passphrases (Entra ID)

---

## Maintenance

### Updating Templates
Templates are stored in this repository. To get the latest:
1. Clone this repository
2. Download the latest template files
3. Re-deploy the container with new template

### Let's Encrypt Certificate Renewal (Public Mode)
- Automatic renewal happens 30 days before expiration
- No manual intervention needed
- Logs are available in `/var/log/apache2` volume

---

## Support

For issues, feature requests, or questions, visit the main repository:
- **GitHub**: https://github.com/auskento/yahlp
- **Documentation**: https://github.com/auskento/yahlp

---

## License

These templates are part of the YAHLP (Yet Another HomeLab Portal) project and follow the same license.
