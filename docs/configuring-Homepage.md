<!--
SPDX-FileCopyrightText: 2020 Aaron Raimist
SPDX-FileCopyrightText: 2020 Chris van Dijk
SPDX-FileCopyrightText: 2020 Dominik Zajac
SPDX-FileCopyrightText: 2020 Mickaël Cornière
SPDX-FileCopyrightText: 2020-2024 MDAD project contributors
SPDX-FileCopyrightText: 2020-2024 Slavi Pantaleev
SPDX-FileCopyrightText: 2022 François Darveau
SPDX-FileCopyrightText: 2022 Julian Foad
SPDX-FileCopyrightText: 2022 Warren Bailey
SPDX-FileCopyrightText: 2023 Antonis Christofides
SPDX-FileCopyrightText: 2023 Felix Stupp
SPDX-FileCopyrightText: 2023 Julian-Samuel Gebühr
SPDX-FileCopyrightText: 2023 Pierre 'McFly' Marty
SPDX-FileCopyrightText: 2024 Thomas Miceli
SPDX-FileCopyrightText: 2024-2026 Suguru Hirahara

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Setting up homepage

This is an [Ansible](https://www.ansible.com/) role which installs [homepage](https://homepage.dev) to run as a [Docker](https://www.docker.com/) container wrapped in a systemd service.

homepage is a highly customizable dashboard for managing your favorite applications and services with a drag-and-drop grid system. It also integrates with various self-hosted applications.

See the project's [documentation](https://homepage.dev/docs/getting-started) to learn what homepage does and why it might be useful to you.

## Prerequisites

Homepage stores its YAML configuration in `homepage_data_path`; it does not require a database.

## Adjusting the playbook configuration

To enable homepage with this role, add the following configuration to your `vars.yml` file.

**Note**: the path should be something like `inventory/host_vars/mash.example.com/vars.yml` if you use the [MASH Ansible playbook](https://github.com/mother-of-all-self-hosting/mash-playbook).

```yaml
########################################################################
#                                                                      #
# homepage                                                               #
#                                                                      #
########################################################################

homepage_enabled: true

########################################################################
#                                                                      #
# /homepage                                                              #
#                                                                      #
########################################################################
```

### Set the hostname

To enable homepage you need to set the hostname as well. To do so, add the following configuration to your `vars.yml` file. Make sure to replace `example.com` with your own value.

```yaml
homepage_hostname: "example.com"
```

After adjusting the hostname, make sure to adjust your DNS records to point the domain to your server.

**Note**: hosting homepage under a subpath (by configuring the `homepage_path_prefix` variable) does not seem to be possible due to homepage's technical limitations.

### Extending the configuration

There are some additional things you may wish to configure about the service.

Take a look at:

- [`defaults/main.yml`](../defaults/main.yml) for some variables that you can customize via your `vars.yml` file. You can override settings (even those that don't have dedicated playbook variables) using the `homepage_environment_variables_additional_variables` variable

See [the official documentation](https://homepage.dev/docs/advanced/environment-variables/) for a complete list of homepage's config options that you could put in `homepage_environment_variables_additional_variables`.

## Installing

After configuring the playbook, run the installation command of your playbook as below:

```sh
ansible-playbook -i inventory/hosts setup.yml --tags=setup-all,start
```

If you use the MASH playbook, the shortcut commands with the [`just` program](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/just.md) are also available: `just install-all` or `just setup-all`

## Usage

After running the command for installation, homepage becomes available at the specified hostname like `https://example.com`.

You can open the page with a web browser to start the onboarding process. See [this official guide](https://homepage.dev/docs/getting-started/after-the-installation/) for details.

## Troubleshooting

### Check the service's logs

You can find the logs in [systemd-journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html) by logging in to the server with SSH and running `journalctl -fu homepage` (or how you/your playbook named the service, e.g. `mash-homepage`).
