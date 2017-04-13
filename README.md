# slapd-base

This role sets up a very basic OpenLDAP slapd with an (almost) empty OLC.
The slapd is not usable, as no database is created.
This has to be done by another role like [slapd-config](https://github.com/stuvusIT/slapd-config).

The main purpose is to clear all distribution-specific settings of the slapd server.

Some things can not be determined by the role (for instance, if the OLC was initialized).
For this purpose, flag files are created and their existence is checked in later playbook executions.

## Requirements

A dpkg- or pacman-based Linux distribution.

## Role Variables

The role allows to set a number of variables.
All variables are required.

| Name                        | Default                     | Description                                                         |
|-----------------------------|-----------------------------|---------------------------------------------------------------------|
| `slapd_run_dir`             | `/run/slapd`                | Runtime directory for args file, pid file and ldapi socket          |
| `slapd_ldapi_socket`        | `{{slapd_run_dir}}/ldapi`   | ldapi unix socket for local slapd administration                    |
| `slapd_mdb_dir`             | `/var/lib/slapd`            | Directory for the mdb. The directory is created, but the mdb is not |
| `slapd_etc_dir`             | `/etc/ldap`                 | slapd configuration in /etc, usually /etc/ldap or /etc/openldap     |
| `slapd_olc_dir`             | `{{slapd_etc_dir}}/slapd.d` | Path where the LDIF files of the OLC reside                         |
| `global_flags_dir`          | `{{slapd_etc_dir}}`         | Path where this role puts flags about what was done                 |
| `slapd_schema_dir`          | `{{slapd_etc_dir}}/schema`  | Path where the default slapd schemas reside                         |
| `slapd_user`                | `openldap`                  | User under which slapd runs                                         |
| `slapd_group`               | `{{slapd_user}}`            | Group under which slapd runs                                        |
| `slapd_olc_rootdn`          | `cn=root,cn=config`         | Rootdn of the OLC                                                   |
| `slapd_olc_rootdn_password` |                             | Password for the OLC rootdn                                         |

All of the variables prefixed with `slapd_` are exposed as facts for other roles.
The only exception is `slapd_olc_rootdn_password` for security purposes.

## Dependencies

None

## Example Playbook

```yml
- hosts: ldap
  roles:
  - slapd-base
    slapd_etc_dir: /etc/openldap
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

## Author Information

- [Janne Heß](https://github.com/dasJ)
