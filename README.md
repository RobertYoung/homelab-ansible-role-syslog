# homelab-ansible-role-syslog

Ansible role for configuring rsyslog to forward logs to a Graylog server over TLS.

## Requirements

- Ansible >= 2.15
- Target: Ubuntu (focal, jammy, noble) or Debian (bullseye, bookworm)
- TLS certificates must be present on target hosts

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `syslog_graylog_url` | `graylog.local.iamrobertyoung.co.uk` | Graylog server hostname |
| `syslog_graylog_port` | `5140` | Graylog syslog input port |
| `syslog_fqdn` | **required** | Fully qualified domain name of the host (used for TLS certificate paths) |

## Usage

### Install via requirements.yml

```yaml
- src: git@github.com:RobertYoung/homelab-ansible-role-syslog.git
  scm: git
  version: main
  name: syslog
```

```bash
ansible-galaxy install -r requirements.yml
```

### Example Playbook

```yaml
- hosts: servers
  roles:
    - role: syslog
      vars:
        syslog_fqdn: "{{ inventory_hostname }}"
        syslog_graylog_url: graylog.example.com
        syslog_graylog_port: 5140
```

## What Gets Installed

- rsyslog with GnuTLS and RELP support
- Configuration to forward all logs to Graylog over TLS (RFC 5424 format)

## TLS Requirements

The role expects the following certificate files to exist on the target host:
- CA certificate: `/etc/ssl/certs/ca-certificates.crt`
- Host certificate: `/etc/ssl/certs/<syslog_fqdn>.crt`
- Host private key: `/etc/ssl/private/<syslog_fqdn>.pem`

## License

MIT
