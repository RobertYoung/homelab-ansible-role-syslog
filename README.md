# homelab-ansible-role-syslog

[![Lint](https://github.com/RobertYoung/homelab-ansible-role-syslog/actions/workflows/lint.yml/badge.svg)](https://github.com/RobertYoung/homelab-ansible-role-syslog/actions/workflows/lint.yml)
[![Release](https://github.com/RobertYoung/homelab-ansible-role-syslog/actions/workflows/release.yml/badge.svg)](https://github.com/RobertYoung/homelab-ansible-role-syslog/actions/workflows/release.yml)
[![SLSA 3](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/RobertYoung/homelab-ansible-role-syslog/badge)](https://scorecard.dev/viewer/?uri=github.com/RobertYoung/homelab-ansible-role-syslog)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

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

## Supply Chain Security

This project implements [SLSA](https://slsa.dev/) Level 3 provenance for release artifacts.

- Provenance attestations are submitted to [GitHub Attestations](https://github.com/RobertYoung/homelab-ansible-role-syslog/attestations)
- Release artifacts include `.intoto.jsonl` provenance files
- Security posture tracked via [OpenSSF Scorecard](https://scorecard.dev/viewer/?uri=github.com/RobertYoung/homelab-ansible-role-syslog)

### Verifying Release Provenance

```bash
# Using GitHub CLI (recommended)
gh attestation verify syslog-<VERSION>.tar.gz \
  --repo RobertYoung/homelab-ansible-role-syslog

# Or using slsa-verifier
VERSION="v1.2.0"  # Replace with desired version
slsa-verifier verify-artifact syslog-${VERSION}.tar.gz \
  --provenance-path syslog-${VERSION}.tar.gz.intoto.jsonl \
  --source-uri github.com/RobertYoung/homelab-ansible-role-syslog \
  --source-tag "${VERSION}"
```

## License

MIT
