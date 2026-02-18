# Contributing to N8N-TailScale

Thank you for your interest in contributing. This guide explains how to report issues, suggest improvements, and submit changes.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Reporting Issues](#reporting-issues)
- [Suggesting Features](#suggesting-features)
- [Development Setup](#development-setup)
- [Submitting Changes](#submitting-changes)
- [Pull Request Guidelines](#pull-request-guidelines)
- [Style Guide](#style-guide)
- [Security Vulnerabilities](#security-vulnerabilities)

---

## Code of Conduct

Be respectful, constructive, and inclusive. Harassment, discrimination, and bad-faith engagement are not tolerated.

---

## Reporting Issues

Before opening an issue, check the [existing issues](https://github.com/mvdmakesthings/N8N-TailScale/issues) to avoid duplicates.

When filing a bug report, include:

- **Environment** — OS, Docker version (`docker --version`), Compose version (`docker compose version`), GPU model, NVIDIA driver version (`nvidia-smi`)
- **Steps to reproduce** — Exact commands you ran
- **Expected behavior** — What you expected to happen
- **Actual behavior** — What actually happened
- **Logs** — Relevant output from `docker compose logs <service>`

```bash
# Collect diagnostic info
docker --version
docker compose version
nvidia-smi
docker compose ps
docker compose logs --tail=50
```

---

## Suggesting Features

Open a [GitHub issue](https://github.com/mvdmakesthings/N8N-TailScale/issues/new) with the label `enhancement`. Describe:

- **The problem** — What limitation or gap you're encountering
- **Proposed solution** — How you'd like it to work
- **Alternatives considered** — Other approaches you evaluated

Feature requests that align with the project's scope (secure, self-hosted, GPU-sharing workflow automation) are most likely to be accepted.

---

## Development Setup

### Prerequisites

- Docker Engine 24.0+
- Docker Compose v2.20+
- NVIDIA Container Toolkit (for GPU testing)
- A Tailscale account (for end-to-end testing)

### Local setup

```bash
git clone git@github.com:mvdmakesthings/N8N-TailScale.git
cd N8N-TailScale
cp .env.example .env
# Fill in .env with your values (see README.md for details)
```

### Validate the Compose file

```bash
docker compose config --quiet
```

This checks for syntax errors without starting services. Warnings about unset environment variables are expected if you haven't filled in `.env` yet.

### Start the stack locally

```bash
docker compose up -d
docker compose logs -f
```

---

## Submitting Changes

### 1. Fork and branch

```bash
git checkout -b your-feature-name
```

Use a descriptive branch name: `fix/ollama-healthcheck`, `feat/amd-gpu-support`, `docs/acl-examples`.

### 2. Make your changes

- Keep changes focused. One logical change per pull request.
- Update documentation if your change affects setup, configuration, or behavior.
- Test locally before submitting.

### 3. Commit

Write clear commit messages:

```
Add healthcheck to Ollama service

The ollama service now exposes a /api/tags healthcheck so n8n can
wait for model availability before starting AI workflows.
```

- First line: imperative mood, under 72 characters
- Body (optional): explain *why*, not just *what*

### 4. Push and open a PR

```bash
git push origin your-feature-name
```

Open a pull request against `main`.

---

## Pull Request Guidelines

- **Title:** Short, descriptive, imperative mood (e.g., "Add AMD GPU support")
- **Description:** Explain what changed and why. Reference related issues with `Closes #123`.
- **Scope:** One concern per PR. Don't bundle unrelated changes.
- **Tests:** If your change affects runtime behavior, describe how you tested it.
- **Docs:** Update `README.md` if you changed configuration, commands, or architecture.

### What we look for in review

- Does the change maintain the security model? (No published ports, no weakened sandboxing, no exposed secrets)
- Is the `docker-compose.yml` still valid? (`docker compose config --quiet`)
- Are new environment variables documented in both `.env.example` and `README.md`?
- Does the change work on a clean clone? (`git clone` → `cp .env.example .env` → `docker compose up -d`)

---

## Style Guide

### Docker Compose

- Use the `docker-compose.yml` filename (Compose v2 default).
- Pin to `latest` tags for simplicity. This is a home-lab stack, not a production deployment.
- Place `networks`, `volumes`, and `services` sections in that order.
- Alphabetize service-level keys (`cap_add`, `cap_drop`, `deploy`, `depends_on`, `devices`, `environment`, `image`, `networks`, `restart`, `security_opt`, `tmpfs`, `volumes`).

### Documentation

- Write in plain English. Avoid jargon where a simpler word works.
- Use fenced code blocks with language identifiers (````bash`, ````yaml`, ````jsonc`).
- Use tables for structured comparisons.
- Prefer concrete examples over abstract descriptions.

### Secrets

- Never commit `.env` files, API keys, auth tokens, or private keys.
- New secrets must be added to `.env.example` with a placeholder value and a comment explaining how to generate or obtain them.

---

## Security Vulnerabilities

**Do not open a public issue for security vulnerabilities.**

If you discover a security issue, please email the maintainer directly or use [GitHub's private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-creating-advisories/privately-reporting-a-security-vulnerability).

Include:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

We will respond within 48 hours and coordinate a fix before public disclosure.
