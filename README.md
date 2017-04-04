# slapd-base

This role sets up a very basic OpenLDAP slapd with an (almost) empty OLC.
The slapd is not usable, as no database is created.
This has to be done by another role.

The main purpose is to clear all distribution-specific settings of the slapd server.

Some things can not be determined by the role (for instance, if the OLC was initialized).
For this purpose, flag files are created and their existence is checked in later playbook executions.

## Requirements

A dpkg-based Linux distribution.

## Role Variables

The role allows to set a number of variables:
- `run_dir` - Default: `/run/slapd` - Runtime directory for args file, pid file and ldapi socket
- `ldapi_socket` - Default: `{{ run_dir }}/ldapi` - ldapi unix socket for local slapd administration
- `db_dir` - Default: `/var/lib/slapd` - Directory for the mdb (which is not created by this roles)
- `etc_dir` - Default: `/etc/ldap` - slapd configuration in /etc, usually /etc/ldap or /etc/openldap
- `olc_dir` - Default: `{{ etc_dir }}/slapd.d` - Path where the LDIF files of the OLC reside
- `flags_dir` - Default: `{{ etc_dir }}` - Path where this role puts flags about what was done
- `schema_dir` - Default: `{{ etc_dir }}/schema` - Path where the default slapd schemas reside
- `slapd_user` - Default: `openldap` - User under which slapd runs
- `slapd_group` - Default: `{{ slapd_user }}` - Group under which slapd runs
- `olc_rootdn` - Default: `cn=root,cn=config` - Rootdn of the OLC
- `olc_rootdn_password` - `Required` - Password for the OLC rootdn

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

CC-BY-SA

## Author Information

- [Janne Heß](https://github.com/dasJ)
