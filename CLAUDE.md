# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Ansible role (`syslog`) for configuring rsyslog to forward logs to a Graylog server over TLS on Ubuntu/Debian systems.

## Development Commands

### Linting
```bash
yamllint .                    # Lint all YAML files
ansible-lint                  # Lint Ansible code (currently disabled in CI)
```

### Pre-commit Hooks
```bash
pre-commit install            # Install hooks
pre-commit run --all-files    # Run all hooks manually
```

### Tool Management
Uses `mise` for tool versions (Ansible 13, pipx 1.8). Run `mise install` to set up.

## Role Structure

- `tasks/main.yml` - Installs rsyslog packages and deploys configuration
- `defaults/main.yml` - Default variables (syslog_graylog_url, syslog_graylog_port)
- `templates/01-graylog.conf.j2` - Rsyslog configuration template for Graylog forwarding

## Required Variables

The `syslog_fqdn` variable must be provided for TLS certificate paths.

## YAML Style

Line length max 120, truthy values must be "true"/"false"/"yes"/"no", 1 space minimum after comments.
