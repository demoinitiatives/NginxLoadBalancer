
# 📦 Docker Compose Setup with NGINX Reverse Proxy

## Overview

This project sets up a simple reverse proxy using **NGINX** to load balance traffic across three demo applications (`app1`, `app2`, `app3`). Each application serves a single-page HTML site, and NGINX forwards incoming requests to one of the backend containers.

---

## 🧱 Architecture

```
Client
  ↓
NGINX (Reverse Proxy)
  ↓
[ app1 | app2 | app3 ]
```

- **NGINX** listens on port `80` and proxies requests to the backend services.
- **Upstream servers** are defined as `app1`, `app2`, and `app3`, each running on port `80`.

---

## 📁 File Structure

```
.
├── docker-compose.yml
├── nginx.conf
├── app1/
│   └── index.html
├── app2/
│   └── index.html
├── app3/
│   └── index.html
└── README.md
```

---

## ⚙️ Configuration Details

### `nginx.conf`

```nginx
events {}

http {
    upstream backend {
        server app1:80;
        server app2:80;
        server app3:80;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

---

## 🚀 Getting Started

1. **Clone the repository**:
   ```bash
   git clone <your-repo-url>
   cd <your-project-folder>
   ```

2. **Build and run the containers**:
   ```bash
   docker-compose up --build
   ```

3. **Access the application**:
   Open your browser and go to `http://localhost`. NGINX will route requests to one of the demo apps.

---

## 🧪 Testing

- Refresh the page multiple times to see traffic routed to different apps.
- You can customize each `index.html` to display which app is serving the request.

---

## 📌 Notes

- Ensure Docker and Docker Compose are installed.
- You can scale the number of backend apps by modifying the `upstream` block and adding more services in `docker-compose.yml`.
