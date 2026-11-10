# Buildable Matomo application stack for Kubernetes on Wodby

Deploy Matomo built from your Git repository on Kubernetes with Wodby: your
repository pins Matomo's version and holds your own plugins.

This repository defines the Wodby stack manifests and default service
composition for buildable Matomo.

<!-- wodby:generated:start -->

## Stack contract

- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [Buildable Matomo service](https://github.com/wodby/service-matomo-buildable)
- [MariaDB service](https://github.com/wodby/service-mariadb)
- [NFS provisioner service](https://github.com/wodby/service-nfs-provisioner)
- [OpenSMTPD service](https://github.com/wodby/service-opensmtpd)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Matomo<br>`matomo` | required; enabled by default; volumes: `files` 5 GB; links: `db` → `mariadb`, `files` → `files-nfs`, `sendmail` → `opensmtpd` |
| MariaDB<br>`mariadb` | optional; enabled by default; volumes: `data` 20 GB |
| Files NFS storage<br>`files-nfs` | optional; enabled by default; volumes: `data` 10 GB |
| OpenSMTPD<br>`opensmtpd` | optional; enabled by default |

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifest

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

<!-- wodby:generated:end -->

## Deploy this stack

Connect a Git repository started from the
[Matomo boilerplate](https://github.com/wodby/matomo-boilerplate):

- `.matomo-version` names the Matomo release to run, such as `5.14.2`. The build
  downloads it, checks its signature and installs it on
  [wodby/matomo-php](https://github.com/wodby/matomo-php).
- `plugins/` holds your own Matomo plugins. They are activated after every
  deployment, and plugins you remove are deactivated.

Upgrading Matomo means changing `.matomo-version` and deploying; the deployment
updates the database. You decide when to upgrade, so watch Matomo's releases for
security fixes. To have Wodby keep Matomo up to date instead, use the
[Matomo stack](https://github.com/wodby/stack-matomo).

Enter the superuser's password and email, and optionally a username and the
address of the first website to track, in the Matomo service's settings when you
create the app. The first deployment installs Matomo with that account; Matomo's
web installer is not used. Reports are archived every hour by the service's cron
job.

Matomo's settings file, custom logos and Marketplace plugins are kept on the
shared `files` volume: back it up together with the database.

## Maintain a custom version

1. Fork this repository.
2. Edit the stack manifest.
3. Import the repository as a [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target
and derivative reference. Stack-local names and referenced service names are
distinct identifiers.
