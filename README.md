# slapd-base

This role sets up a very basic OpenLDAP slapd with an (almost) empty OLC.
The slapd is not usable, as no database is created.
This has to be done by another role.

The main purpose is to clear all distribution-specific settings of the slapd server.

Some things can not be determined by the role (for instance, if the OLC was initialized).
For this purpose, flag files are created and their existence is checked in later playbook executions.

## Requirements

A dpkg- or pacman-based Linux distribution.

## Role Variables

The role allows to set a number of variables:
| Name                  | Required           | Default                 | Description                                                         |
|-----------------------|--------------------|-------------------------|---------------------------------------------------------------------|
| `run_dir`             | :heavy_check_mark: | `/run/slapd`            | Runtime directory for args file, pid file and ldapi socket          |
| `ldapi_socket`        | :heavy_check_mark: | `{{ run_dir }}/ldapi`   | ldapi unix socket for local slapd administration                    |
| `db_dir`              | :heavy_check_mark: | `/var/lib/slapd`        | Directory for the mdb. The directory is created, but the mdb is not |
| `etc_dir`             | :heavy_check_mark: | `/etc/ldap`             | slapd configuration in /etc, usually /etc/ldap or /etc/openldap     |
| `olc_dir`             | :heavy_check_mark: | `{{ etc_dir }}/slapd.d` | Path where the LDIF files of the OLC reside                         |
| `flags_dir`           | :heavy_check_mark: | `{{ etc_dir }}`         | Path where this role puts flags about what was done                 |
| `schema_dir`          | :heavy_check_mark: | `{{ etc_dir }}/schema`  | Path where the default slapd schemas reside                         |
| `slapd_user`          | :heavy_check_mark: | `openldap`              | User under which slapd runs                                         |
| `slapd_group`         | :heavy_check_mark: | `{{ slapd_user }}`      | Group under which slapd runs                                        |
| `olc_rootdn`          | :heavy_check_mark: | `cn=root,cn=config`     | Rootdn of the OLC                                                   |
| `olc_rootdn_password` | :heavy_check_mark: |                         | Password for the OLC rootdn                                         |

All of these variables are exposed as facts for other roles.
The only exception is `olc_rootdn_password` for security purposes.

## Dependencies

None

## Example Playbook

```yml
- hosts: ldap
  roles:
  - slapd-base
    etc_dir: /etc/openldap
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

## Author Information

- [Janne Heß](https://github.com/dasJ)
