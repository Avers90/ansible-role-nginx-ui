# Ansible Role: Nginx UI

Install and configure [Nginx UI](https://github.com/0xJacky/nginx-ui) - Yet another WebUI for Nginx.

## Features

- Online statistics for server indicators (CPU, memory, disk usage)
- One-click deployment and automatic renewal of Let's Encrypt certificates
- Online editing of Nginx configurations with syntax highlighting
- Online view of Nginx logs
- Web Terminal
- Dark Mode

## Requirements

- Debian 11+ / Ubuntu 20.04+
- Nginx (installed automatically)

## Role Variables

### Basic Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `nginx_ui_version` | `v2.3.2` | Nginx UI version |
| `nginx_ui_install_dir` | `/usr/local/nginx-ui` | Installation directory |
| `nginx_ui_config_dir` | `/usr/local/etc/nginx-ui` | Configuration directory |
| `nginx_ui_log_dir` | `/var/log/nginx-ui` | Log directory |

### Server Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `nginx_ui_host` | `0.0.0.0` | Listen address |
| `nginx_ui_port` | `9000` | Listen port |
| `nginx_ui_localhost_only` | `false` | Bind to localhost only (access via SSH tunnel) |
| `nginx_ui_run_mode` | `release` | Run mode: `debug`, `release` |

### HTTPS Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `nginx_ui_enable_https` | `false` | Enable HTTPS for panel |
| `nginx_ui_enable_h2` | `false` | Enable HTTP/2 |
| `nginx_ui_enable_h3` | `false` | Enable HTTP/3 |

### Certificate Settings (Let's Encrypt)

| Variable | Default | Description |
|----------|---------|-------------|
| `nginx_ui_cert_email` | `""` | Email for Let's Encrypt notifications |
| `nginx_ui_cert_renewal_interval` | `7` | Days before expiry to renew |
| `nginx_ui_cert_http_challenge_port` | `9180` | HTTP challenge port |

### Auth Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `nginx_ui_ip_whitelist` | `""` | IP whitelist (comma-separated) |
| `nginx_ui_ban_threshold_minutes` | `10` | Ban duration after max attempts |
| `nginx_ui_max_attempts` | `10` | Max failed login attempts |

### OpenAI Settings (Optional)

| Variable | Default | Description |
|----------|---------|-------------|
| `nginx_ui_openai_base_url` | `""` | OpenAI API base URL |
| `nginx_ui_openai_token` | `""` | OpenAI API token |
| `nginx_ui_openai_model` | `gpt-4o` | Model to use |
| `nginx_ui_enable_code_completion` | `false` | Enable code completion |

### Service Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `nginx_ui_service_name` | `nginx-ui` | Systemd service name |
| `nginx_ui_service_enabled` | `true` | Enable service on boot |
| `nginx_ui_service_state` | `started` | Service state |

## Dependencies

None.

## Example Playbook

### Basic Installation

```yaml
- hosts: webservers
  roles:
    - role: nginx-ui
```

### Secure Installation (localhost only)

```yaml
- hosts: webservers
  roles:
    - role: nginx-ui
      vars:
        nginx_ui_localhost_only: true
        nginx_ui_cert_email: "admin@example.com"
```

### With OpenAI Integration

```yaml
- hosts: webservers
  roles:
    - role: nginx-ui
      vars:
        nginx_ui_openai_base_url: "https://api.openai.com"
        nginx_ui_openai_token: "{{ vault_openai_token }}"
        nginx_ui_enable_code_completion: true
```

## Access

### Direct Access

```
http://<server_ip>:9000
```

### Via SSH Tunnel (when `nginx_ui_localhost_only: true`)

```bash
ssh -L 9000:127.0.0.1:9000 user@server
```

Then open http://localhost:9000 in your browser.

### First Time Setup

On first access, you will be prompted to create an admin account.

## Management Commands

```bash
# Start service
systemctl start nginx-ui

# Stop service
systemctl stop nginx-ui

# Restart service
systemctl restart nginx-ui

# View status
systemctl status nginx-ui

# View logs
journalctl -u nginx-ui -f
```

## File Locations

| Path | Description |
|------|-------------|
| `/usr/local/nginx-ui/nginx-ui` | Binary |
| `/usr/local/etc/nginx-ui/app.ini` | Configuration |
| `/usr/local/etc/nginx-ui/database.db` | SQLite database |
| `/var/log/nginx-ui/` | Logs |

## Updating

To update Nginx UI, change the version and run the playbook:

```yaml
nginx_ui_version: "v2.3.3"
```

```bash
ansible-playbook playbook.yml --tags nginx-ui
```

## License

MIT

## Author

Avers90
