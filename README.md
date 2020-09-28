# Ansible Role: logrotate

An Ansible Role that configures a log rotation service.

[![Actions Status](https://github.com/tristan-weil/ansible-role-logrotate/workflows/molecule/badge.svg?branch=master)](https://github.com/tristan-weil/ansible-role-logrotate/actions)

## Role Variables

Available variables are listed below, (see also `defaults/main.yml`).

### logrotate

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| logrotate_configs_list | [] | a list of <*logrotate entry*> |

### <*logrotate entry*>

A *logrotate entry* represents the parameters used to configure a logrotate configuration.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| log_fic_path  | the path to the logfile (or a list of pathes) |
| log_fic_mode  | the mode of the new logfile |
| archives_count | the number of archives to keep |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| state         | present | *present / absent*: add or delete the configuration |
| log_fic_user  |         | the user of the logfile (or keep the used one) |
| log_fic_group |         | the group of the logfile (or keep the used one) |
| when          |         | the delay before rotation, see valid values are in `vars/main.yml` |
| compress      | True    | *True / False*: compress to the archived logfile
| maxsize_in_bytes |      | the max size in bytes of the logfile before rotation
| mail          |         | the mail to contact after rotation
| command       |         | the command to execute after rotation
| binary        | False   | *True / False*: enable binary mode (only OpenBSD)

## Example Playbook

    - hosts: 'webservers'
      roles:
        - role: 'ansible-role-logrotate'
          logrotate_configs_list:
            - name: 'testinfra1'
              log_fic_path: /testinfra1.log
              log_fic_user: vagrant
              log_fic_group: vagrant
              log_fic_mode: '0640'
              compress: True
              archives_count: 42
              when: daily
              binary: True

            - name: 'testinfra2'
              log_fic_path:
                - /testinfra2a.log
                - /testinfra2b.log
              log_fic_user: vagrant
              log_fic_group: vagrant
              log_fic_mode: '0640'
              compress: True
              archives_count: 42
              when: daily
              command: "echo ohai"

## Todo

None.

## Dependencies

See [requirements_galaxy.yml](https://github.com/tristan-weil/ansible-role-logrotate/blob/master/requirements_galaxy.yml)

## Supported platforms

See [meta/main.yml](https://github.com/tristan-weil/ansible-role-logrotate/blob/master/meta/main.yml)

## License

See [LICENSE.md](https://github.com/tristan-weil/ansible-role-logrotate/blob/master/LICENSE.md)
