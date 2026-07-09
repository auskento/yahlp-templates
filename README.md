# YAHLP - Yet Another HomeLab Portal - Unraid Templates

![YAHLP Logo](https://raw.githubusercontent.com/auskento/YAHLP/main/yahlp.png)

Official Unraid container templates for **YAHLP** (Yet Another HomeLab Portal), a comprehensive reverse proxy and unified media server dashboard.

These templates are part of the [Unraid Community Applications](https://unraid.net/community/apps) ecosystem.

## Overview

YAHLP (Yet Another HomeLab Portal) is a production-ready reverse proxy and unified media server dashboard that supports:
- **19 services** (Sonarr, Radarr, Lidarr, Whisparr, Prowlarr, Jackett, Seerr, Bazarr, SABnzbd, NZBGet, NZBHydra, Transmission, qBittorrent, Deluge, Jellyfin, Emby, Plex, Tautulli, Maintainerr)
- **5 responsive layouts** (Classic, Modern, Sleek, Minimal, Mobile-optimized)
- **Multiple authentication methods** (None, Basic Auth, Google OAuth2, Entra ID)
- **Public and Private deployment modes**
- **Automatic HTTPS** (Let's Encrypt) for public deployments
- **Real-time service health monitoring**
- **Customizable service ordering and theming**
- **3-digit service codes** for easy dashboard configuration

## Available Templates

Choose the template that matches your deployment scenario:

### 1. **Private Mode with Basic Auth**
**File:** `yahlp-private-basic.xml`

**Best for:** Internal-only deployments on a trusted local network

**Features:**
- HTTP only (no SSL needed)
- Basic authentication (username/password)
- Internal IP address access (e.g., `http://192.168.1.100`)
- No certificate management
- Minimal resource usage
- Perfect for home labs and testing

**Required Configuration:**
- `IP`: Your server IP address (e.g., `192.168.1.100`)
- `AUTHTYPE`: Set to `basic`
- `BASIC_AUTH_CREDENTIALS`: Username and password (format: `user:pass` or `user1:pass1|user2:pass2`)

**Optional Configuration:**
- `DASHBOARD_NAME`: Custom dashboard title
- `DASHBOARD_STYLE`: Layout choice (classic, modern, sleek, minimal)
- `DASHBOARD_COLOR`: Accent color in hex format
- `DASHBOARD_LANDING`: Default page to load
- `DASHBOARD_ORDER`: Service display order (see service codes reference)

---

### 2. **Public Mode with Basic Auth**
**File:** `yahlp-public-basic.xml`

**Best for:** External access with username/password protection

**Features:**
- HTTPS via Let's Encrypt (automatic SSL certificates)
- Basic authentication (username/password)
- Full domain-based access (e.g., `https://yourdomain.com`)
- Automatic certificate renewal
- Ideal for small deployments or additional security layer

**Required Configuration:**
- `DOMAIN`: Your domain name (e.g., `yourdomain.com`)
- `EMAIL`: Email for Let's Encrypt notifications
- `AUTHTYPE`: Set to `basic`
- `BASIC_AUTH_CREDENTIALS`: Username and password (format: `user:pass` or `user1:pass1|user2:pass2`)
- **Network:** Port forwarding for ports 80 and 443 to your Unraid server

**Optional Configuration:**
- Dashboard settings (name, style, color, landing page, service order)
- `DASHBOARD_TEST`: Set to `false` when ready for production certificates

**Setup Requirements:**
1. Add DNS A record pointing your domain to your server IP
2. Ensure ports 80 and 443 are port-forwarded from your router
3. Set `DASHBOARD_TEST=false` once ready for production (avoids Let's Encrypt rate limiting)

---

### 3. **Public Mode with Google OAuth**
**File:** `yahlp-public-google.xml`

**Best for:** External access protected by Google account authentication

**Features:**
- HTTPS via Let's Encrypt (automatic SSL certificates)
- Google OAuth2 authentication (requires Google Cloud setup)
- Full domain-based access (e.g., `https://yourdomain.com`)
- Automatic certificate renewal
- More secure than Basic Auth

**Required Configuration:**
- `DOMAIN`: Your domain name (e.g., `yourdomain.com`)
- `EMAIL`: Email for Let's Encrypt notifications
- `AUTHTYPE`: Set to `google`
- `GOOGLE_CLIENT_ID`: From Google Cloud Console
- `GOOGLE_CLIENT_SECRET`: From Google Cloud Console
- `GOOGLE_REDIRECT_URI`: OAuth callback URL (e.g., `https://yourdomain.com/auth/oauth2/callback`)
- **Network:** Port forwarding for ports 80 and 443 to your Unraid server

**Setup Steps:**
1. Create a project in [Google Cloud Console](https://console.cloud.google.com)
2. Create OAuth 2.0 credentials (Web application type)
3. Add authorized redirect URI: `https://yourdomain.com/auth/oauth2/callback`
4. Copy Client ID and Secret to the template
5. Add DNS A record for your domain
6. Ensure ports 80 and 443 are port-forwarded
7. Set `DASHBOARD_TEST=false` when ready for production

---

### 4. **Public Mode with Entra ID (Microsoft) OAuth**
**File:** `yahlp-public-entra.xml`

**Best for:** External access protected by Microsoft/Azure AD authentication (enterprise deployments)

**Features:**
- HTTPS via Let's Encrypt (automatic SSL certificates)
- Entra ID (Azure AD) OAuth2 authentication
- Full domain-based access (e.g., `https://yourdomain.com`)
- Automatic certificate renewal
- Enterprise-grade authentication

**Required Configuration:**
- `DOMAIN`: Your domain name (e.g., `yourdomain.com`)
- `EMAIL`: Email for Let's Encrypt notifications
- `AUTHTYPE`: Set to `entra`
- `ENTRA_CLIENT_ID`: Azure AD application ID
- `ENTRA_CLIENT_SECRET`: Azure AD application secret
- `ENTRA_TENANT_ID`: Your Azure Tenant ID
- `ENTRA_REDIRECT_URI`: OAuth callback URL (e.g., `https://yourdomain.com/auth/oauth2/callback`)
- `ENTRA_PROVIDER_METADATA_URL`: OpenID configuration URL (format: `https://login.microsoftonline.com/YOUR_TENANT_ID/v2.0/.well-known/openid-configuration`)
- `ENTRA_CRYPTO_PASSPHRASE`: Session encryption key (auto-generated if left blank)
- **Network:** Port forwarding for ports 80 and 443 to your Unraid server

**Setup Steps:**
1. Go to [Azure Portal](https://portal.azure.com)
2. Navigate to Azure AD → App registrations
3. Register a new application
4. Configure OAuth 2.0 redirect URI: `https://yourdomain.com/auth/oauth2/callback`
5. Create a client secret
6. Get your Tenant ID from app settings
7. Get OpenID configuration URL from app settings
8. Copy credentials to the template
9. Add DNS A record for your domain
10. Ensure ports 80 and 443 are port-forwarded
11. Set `DASHBOARD_TEST=false` when ready for production

---

### Master Template (Advanced Users)
**File:** `yahlp.xml`

Comprehensive template showing **all available variables and configuration options**. Use this for:
- Advanced custom configurations
- Per-service URL and API key management
- Fine-grained control over dashboard settings
- Integration with automation systems
- Complete reference documentation

This template includes all 19 services with individual enable/disable and URL configuration for each.

---

## Service Codes Reference

All services use 3-letter codes for dashboard configuration. Use these codes in the `DASHBOARD_ORDER` setting to customize your dashboard layout:

| Code | Service | Category |
|------|---------|----------|
| **SAB** | SABnzbd | USENET |
| **GET** | NZBGet | USENET |
| **HYD** | NZBHydra | USENET |
| **TRA** | Transmission | TORRENTS |
| **QBI** | qBittorrent | TORRENTS |
| **DEL** | Deluge | TORRENTS |
| **PRO** | Prowlarr | SEARCH |
| **JAC** | Jackett | SEARCH |
| **SON** | Sonarr | CONTENT |
| **RAD** | Radarr | CONTENT |
| **LID** | Lidarr | CONTENT |
| **WHI** | Whisparr | CONTENT |
| **SEE** | Seerr | INFRA |
| **BAZ** | Bazarr | INFRA |
| **TAU** | Tautulli | INFRA |
| **MNT** | Maintainerr | INFRA |
| **JEL** | Jellyfin | MEDIA |
| **PLX** | Plex | MEDIA |
| **EMB** | Emby | MEDIA |

**Example DASHBOARD_ORDER configurations:**
```
# Default order (all services)
DASHBOARD_ORDER=SAB,GET,HYD,TRA,QBI,DEL,PRO,JAC,SON,RAD,LID,WHI,SEE,BAZ,TAU,MNT,JEL,PLX,EMB

# Media servers first
DASHBOARD_ORDER=JEL,PLX,EMB,TAU,MNT,SON,RAD,LID,WHI,PRO,JAC,SEE,BAZ,SAB,GET,HYD,TRA,QBI,DEL

# Downloads first
DASHBOARD_ORDER=SAB,GET,HYD,TRA,QBI,DEL,SON,RAD,LID,WHI,PRO,JAC,SEE,BAZ,TAU,MNT,JEL,PLX,EMB

# Minimal setup (just media and content)
DASHBOARD_ORDER=JEL,PLX,EMB,TAU,MNT,SON,RAD
```

---

## Quick Start Guide

### Step 1: Choose Your Template
| Use Case | Template | Auth | SSL |
|----------|----------|------|-----|
| Home lab / Private network | `yahlp-private-basic.xml` | Basic | HTTP |
| External access / Simple auth | `yahlp-public-basic.xml` | Basic | HTTPS |
| External access / Google | `yahlp-public-google.xml` | Google OAuth | HTTPS |
| Enterprise / Microsoft | `yahlp-public-entra.xml` | Entra ID | HTTPS |
| All options visible | `yahlp.xml` (master) | Configurable | Configurable |

### Step 2: Deploy in Unraid
1. Go to `Docker` → `Add Container`
2. Click `Select a template`
3. Paste the template URL or upload the file
4. Configure the required settings (see template section above)
5. Click `Apply`

### Step 3: Configure Services
Once running, configure your backend services:
- Enable services you want to use
- Verify service URLs match your network setup
- Set authentication credentials for each service

### Step 4: Verify
- Access the dashboard via the configured URL
- Verify all enabled services are responding
- Check logs if services don't appear

---

## Configuration Details

### Environment Variables
All templates use environment variables for configuration. Variables are set through:
1. **Unraid Docker UI** - Through the template form fields
2. **Direct env vars** - When deploying via command line
3. **Configuration file** - Via mounted `yahlp.json5` configuration

### Dashboard Customization
All templates support customization through these variables:
- **DASHBOARD_NAME**: Custom title for your dashboard
- **DASHBOARD_STYLE**: Visual layout (classic, modern, sleek, minimal)
- **DASHBOARD_COLOR**: Accent color in hex format (e.g., `#00A99D`)
- **DASHBOARD_LANDING**: Default page on load (e.g., `sonarr`, `radarr/upcoming`)
- **DASHBOARD_ORDER**: Service display order using 3-letter codes

### Supported Deployments

**All 19 Services Supported:**

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
- Whisparr (Adult Content)

**Search/Requests:**
- Prowlarr (Indexer Manager)
- Jackett (Indexer Aggregator)
- Seerr (Request Manager)
- Bazarr (Subtitle Manager)

**Media Streaming:**
- Emby
- Plex
- Jellyfin
- Tautulli (Analytics)
- Maintainerr (Media Maintenance)

---

## Troubleshooting

### Let's Encrypt Certificate Issues (Public Mode)
- Ensure ports 80 and 443 are port-forwarded correctly
- Verify your domain DNS is pointing to your server IP
- Check container logs for certbot/certificate errors
- Ensure your domain is accessible from the internet
- For test mode, remove the `TEST_MODE` variable or set to `false`

### Authentication Not Working
- Verify authentication credentials are set correctly in template
- For OAuth (Google/Entra):
  - Ensure redirect URIs match exactly in provider console
  - Verify Client ID and Secret are correct
  - Check that OAuth application is properly configured
- For Basic Auth:
  - Use format: `user:pass` or `user1:pass1|user2:pass2`

### Services Not Responding
- Verify backend service URLs are correct for your network
- Ensure services are running and accessible from the container
- Check firewall rules between container and services
- Verify all required services have proper URLs configured

### Dashboard Not Loading
- Check container logs for startup errors
- Verify dashboard configuration variables are set
- Clear browser cache and try again
- For custom dashboard colors, ensure valid hex format

### Performance Issues
- Consider using lighter dashboard styles (sleek, minimal)
- Disable unused services to reduce proxy overhead
- Check Unraid container resource limits
- Monitor CPU/Memory usage in Unraid dashboard

---

## Security Considerations

### Private Mode
- Basic Auth is suitable only for trusted networks
- Consider VPN for external access instead of direct exposure
- HTTP only (no encryption by default)
- Suitable for: Home labs, internal networks

### Public Mode - Basic Auth
- Basic Auth sends credentials in Base64 (not secure if HTTPS fails)
- Good for: Simple internal deployments exposed externally
- Recommendation: Use OAuth2 for better security

### Public Mode - OAuth2 (Google/Entra)
- OAuth2 is more secure than Basic Auth
- Credentials never transmitted to YAHLP
- Choose OAuth provider based on your trust model
- Enterprise choice: Entra ID for Microsoft 365 organizations
- Consumer choice: Google OAuth for individual users

### Best Practices
- Enable HTTPS (provided in all public templates via Let's Encrypt)
- Use strong, unique credentials
- Keep OAuth secrets confidential
- Regularly review service access logs
- Use subdomain routing for critical services when possible
- Consider using a Web Application Firewall (WAF) for public deployments

---

## Maintenance

### Updating to Latest Templates
1. Download the latest template files from this repository
2. Update the container with the new template
3. Verify configuration is preserved
4. Check logs for any errors

### Let's Encrypt Certificate Renewal (Public Mode)
- Automatic renewal happens 30 days before expiration
- No manual intervention needed
- Renewal logs available in container logs
- Keep port 80 accessible for renewal validation

### Monitoring
- Check container logs regularly: `docker logs yahlp`
- Monitor certificate renewal (happens automatically)
- Verify service health via dashboard
- Check access logs in `/var/log/apache2` volume mount

---

## Support & Documentation

For more information, visit the main YAHLP repository:
- **GitHub**: https://github.com/auskento/YAHLP
- **Documentation**: https://github.com/auskento/YAHLP/tree/main/docs
- **Issues**: https://github.com/auskento/YAHLP/issues

### Documentation Files
- **Installation**: Setup instructions for different environments
- **Configuration**: Deep dive into all configuration options
- **Authentication**: Detailed OAuth2 setup guides
- **Architecture**: System design and component overview
- **Security**: Security best practices and hardening

---

## License

These templates are part of the YAHLP (Yet Another HomeLab Portal) project. See the main repository for license details.
