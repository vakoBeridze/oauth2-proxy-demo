# OAuth2 Proxy with Keycloak - Secured Static Site

A complete Docker Compose setup demonstrating how to secure static web content (HTML, PDFs, images, etc.) using OAuth2
Proxy and Keycloak for authentication, without modifying your existing HTML/JS files.

## 🎯 Overview

This project demonstrates how to:

- Protect static resources (HTML, CSS, JS, PDFs, images) with OAuth2/OIDC authentication
- Use Keycloak as an Identity Provider (IdP)
- Deploy everything locally with Docker Compose
- Require zero modifications to your existing static files

### Components

| Component        | Version | Purpose                                            | Links                                                                                                                                  |
|------------------|---------|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| **OAuth2 Proxy** | v7.5.1  | Reverse proxy providing OAuth2/OIDC authentication | [GitHub](https://github.com/oauth2-proxy/oauth2-proxy) • [Docs](https://oauth2-proxy.github.io/oauth2-proxy/)                          |
| **Keycloak**     | 23.0    | Open-source Identity and Access Management         | [Website](https://www.keycloak.org/) • [Docs](https://www.keycloak.org/documentation) • [GitHub](https://github.com/keycloak/keycloak) |
| **Nginx**        | Alpine  | Web server serving static content                  | [Website](https://nginx.org/) • [Docs](https://nginx.org/en/docs/)                                                                     |

## 🏗️ Architecture

```
┌─────────┐      ┌──────────────┐      ┌───────────┐      ┌───────────────┐
│ Browser │ ───> │ OAuth2 Proxy │ ───> │   Nginx   │ ───> │ Static Files  │
└─────────┘      └──────────────┘      └───────────┘      └───────────────┘
                        │
                        │ Auth
                        ▼
                  ┌──────────┐
                  │ Keycloak │
                  └──────────┘
```

**Flow:**

1. User requests a resource (e.g., `/documents/file.pdf`)
2. OAuth2 Proxy checks for valid authentication
3. If not authenticated, redirects to Keycloak login
4. User authenticates with Keycloak
5. Keycloak redirects back to OAuth2 Proxy with token
6. OAuth2 Proxy validates token and proxies request to Nginx
7. Nginx serves the protected static content

## 🛠️ Installation & Setup

### 1. Project Structure

Create the following directory structure:

```
.
├── docker-compose.yml
├── nginx.conf
├── html/
│   ├── index.html
│   ├── documents/
│   │   ├── sample.pdf
│   │   └── report.pdf
│   └── images/
│       └── photo.jpg
└── keycloak/
    └── realms/
        └── demo-realm.json (optional)
```

### 2. Start Services

```bash
# Start all services
docker compose up -d

# View logs
docker compose logs -f

# Check status
docker compose ps
```

**Services will be available at:**

- 🔒 **Secured App**: http://localhost:4180
- 🔑 **Keycloak Admin**: http://localhost:8081
- 📄 **Nginx Direct**: http://localhost:8080 (unsecured)

### 3. Test the Setup

1. Open http://localhost:4180
2. Login with `test` / `test`
3. Access protected resources
