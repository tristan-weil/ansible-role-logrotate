# Ansible Role: logrotate

An Ansible Role that configures the logs rotation system.

**NOTE**: only common parameters for both `logrotate` (Linux) and `newsyslog` (OpenBSD) are available.
The objective of this role is to stay simple.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml` and `vars/main.yml`):

    logrotate_config_state: present

    logrotate_logfile: [mandatory]      # the path to the logfile
    logrotate_owner: [optional]         # the owner of the new logfile
    logrotate_group: [optional]         # the group of the new logfile
    logrotate_mode: "640"               # the mode of the new logfile
    logrotate_archives_count: 7         # the number of archives to keep
    logrotate_maxsize: [optional]       # the max size of the logfile before rotation
    logrotate_when: weekly              # the delay before rotation (valid values are in `vars/main.yml`)
    logrotate_compress: True            # True|False True to compress to the archived logfile
    logrotate_mail: [optional]          # the mail to contact after rotation
    logrotate_command: [optional]       # the command to execute after rotation
    
    # newsyslog specific
    logrotate_binary: False             # True|False True if the file is in binary

    # logrotate specific
    logrotate_config_name: [mandatory]  # the config name

The variables to configure the log rotation behaviour.

## Dependencies

- t18s.fr_pkg

## Example Playbook

    - hosts: webservers
      roles:
        - role: t18s.fr_logrotate
          logrotate_config_name: webserver
          logrotate_logfile: /var/log/webserver/access.log
          logrotate_archives_count: 7
          logrotate_when: daily
          logrotate_maxsize: 268435456 # 256M
          logrotate_compress: True
          logrotate_command: "pkill -HUP -u www-data -U www-data -x httpd"

## Todo

None.

## License

```
Copyright (c) 2018 Tristan Weil <titou@lab.t18s.fr>

Permission to use, copy, modify, and distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
```