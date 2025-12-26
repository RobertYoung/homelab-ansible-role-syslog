# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Ansible role (`syslog`) for configuring rsyslog to forward logs to a Graylog server over TLS on Ubuntu/Debian systems.

## Development Commands

### Linting
```bash
yamllint .                    # Lint all YAML files
ansible-lint                  # Lint Ansible code
pre-commit run --all-files    # Run all pre-commit hooks
```

### Testing with Molecule
```bash
molecule test                 # Run full test suite (create, converge, verify, destroy)
molecule converge             # Apply role to test container
molecule verify               # Run verification tests only
molecule login                # SSH into test container for debugging
molecule destroy              # Clean up test containers
```

### Tool Management
Uses `mise` for tool versions. Run `mise install` to set up, then `pip install -r requirements.txt` for Molecule.

## Role Structure

- `tasks/main.yml` - Installs rsyslog packages and deploys configuration
- `handlers/main.yml` - Rsyslog restart handler (triggered by config changes)
- `defaults/main.yml` - Default variables (syslog_graylog_url, syslog_graylog_port)
- `templates/01-graylog.conf.j2` - Rsyslog configuration template for TLS forwarding to Graylog
- `molecule/default/` - Molecule test configuration and verification playbooks

## Required Variables

The `syslog_fqdn` variable must be provided - used for TLS certificate paths at `/etc/ssl/certs/<fqdn>.crt` and `/etc/ssl/private/<fqdn>.pem`.

## YAML Style

Line length max 120, truthy values must be `true`/`false` or `yes`/`no`, 1 space minimum after comments.
