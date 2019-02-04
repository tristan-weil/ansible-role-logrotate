# Ansible Role: logrotate

An Ansible Role that configures a log rotation service.

**NOTE**: only common parameters for both `logrotate` (Linux) and `newsyslog` (OpenBSD) are available.
The objective of this role is to stay simple.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml` and `vars/main.yml`):

    logrotate_config_state: present             # present|absent

    logrotate_log_path: [mandatory]              # the path to the logfile (or a list of pathes if `logrotate` is used)
    logrotate_user: [optional]                  # the owner of the new logfile
    logrotate_group: [optional]                 # the group of the new logfile
    logrotate_mode: "640"                       # the mode of the new logfile
    logrotate_archives_count: 7                 # the number of archives to keep
    logrotate_maxsize_in_bytes: [optional]      # the max size in bytes of the logfile before rotation
    logrotate_when: weekly                      # the delay before rotation, see valid values are in `vars/main.yml` (or a newsyslog delay format if `newsyslog` is used)
    logrotate_compress: True                    # True|False True to compress to the archived logfile
    logrotate_mail: [optional]                  # the mail to contact after rotation
    logrotate_command: [optional]               # the command to execute after rotation
    
    # newsyslog specific
    logrotate_binary: False                     # True|False True if the file is in binary

    # logrotate specific
    logrotate_config_name: [mandatory]          # the config name

The variables to configure the log rotation behaviour.

## Dependencies

- t18s.fr_pkg

## Example Playbook

    - hosts: webservers
      roles:
        - role: t18s.fr_logrotate
          logrotate_config_name: webserver
          logrotate_log_path: /var/log/webserver/access.log
          logrotate_archives_count: 7
          logrotate_when: daily
          logrotate_maxsize_in_bytes: 268435456 # 256M
          logrotate_compress: True
          logrotate_command: "pkill -HUP -u www-data -U www-data -x httpd"

## Todo

Make it available for OpenBSD.

## License

```
Copyright (c) 2018, 2019 Tristan Weil <titou@lab.t18s.fr>

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
