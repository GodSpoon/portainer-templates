# Portainer Templates

A curated collection of Docker Compose templates for easy deployment in Portainer.

## 🚀 Quick Start

### App Templates (Bulk Import)
1. In Portainer: **Settings** → **App Templates**
2. Set URL: `https://raw.githubusercontent.com/GodSpoon/portainer-templates/master/templates.json`
3. Save and deploy from **App Templates**

### Custom Templates (Individual)
1. In Portainer: **Templates** → **Custom Templates** → **Add Custom Template**
2. Choose **Git repository**:
   - **Repository URL**: `https://github.com/GodSpoon/portainer-templates`
   - **Compose Path**: `stacks/[template-name]/docker-compose.yml`

## 📋 Available Templates

| Template | Description | Path |
|----------|-------------|------|
| **qBittorrent VPN** | Secure BitTorrent client with VPN, Privoxy & SOCKS proxy | `stacks/qbittorrent-vpn/` |

[→ View all templates](INDEX.md)

## 📁 Repository Structure

```
├── templates.json              # App Templates definition
├── stacks/                     # Individual Custom Templates
│   └── qbittorrent-vpn/
│       ├── docker-compose.yml
│       ├── .env.example
│       └── README.md
└── icons/                      # Template icons
```

## ✨ Features

- **Multiple Import Methods**: App Templates or individual Custom Templates
- **Environment Variables**: Comprehensive `.env.example` files for easy customization
- **Documentation**: Detailed setup guides for each template
- **Security Focused**: Best practices and secure defaults
- **Health Checks**: Built-in container monitoring

## 🤝 Contributing

1. Create template in `stacks/template-name/`
2. Include `docker-compose.yml`, `.env.example`, and `README.md`
3. Update `INDEX.md` and optionally `templates.json`
4. Submit pull request

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/GodSpoon/portainer-templates/issues)
- **Documentation**: Each template includes detailed README
- **Template Index**: [INDEX.md](INDEX.md)

---

**⚠️ Security Note**: Always review templates before deployment and update default passwords.
