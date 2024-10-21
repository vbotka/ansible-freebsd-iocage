# freebsd_iocage

[![Build Status](https://app.travis-ci.com/vbotka/ansible-freebsd-iocage.svg?branch=master)](https://app.travis-ci.com/vbotka/ansible-freebsd-iocage)
[![GitHub tag](https://img.shields.io/github/v/tag/vbotka/ansible-freebsd-iocage)](https://github.com/vbotka/ansible-freebsd-iocage/tags)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_iocage/) FreeBSD. Install, configure, and start iocage.

Feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-iocage/issues).

[Contributions are welcome](https://github.com/firstcontributions/first-contributions).

By default, the role installs *iocage*, tests sanity, and starts the *iocage*
service. Optionally, you can activate *iocage* and update the default configuration.


## Requirements

### Collections

The collections *ansible.posix* and *ansible.utils* are needed by the recommended optional
role *vbotka.freebsd_postinstall*

* [ansible.posix](https://github.com/ansible-collections/ansible.posix)
* [ansible.utils](https://github.com/ansible-collections/ansible.utils)
* [community.general](https://github.com/ansible-collections/community.general)

### Recommended roles

* Use [vbotka.freebsd_postinstall](https://github.com/vbotka/ansible-freebsd-postinstall)
  to configure: sysctl.conf, loader.conf, fstab, ZFS ...

### Packages

Controller:

* [jc](https://pypi.org/project/jc/) Converts the output of popular command-line tools and
  file-types to JSON. Required by
  [community.general.jc](https://docs.ansible.com/ansible/latest/collections/community/general/jc_filter.html)

Remote hosts:

* FreeBSD package or port sysutils/iocage. If missing, the package, or port will be installed by
  this role.


## Variables

See the defaults and examples in vars.

* By default, the activation of *iocage* is disabled. Optionally, fit
  the ZFS pool to your needs

```yaml
freebsd_iocage_zfs_pool: zroot
freebsd_iocage_zfs_ds: /zroot/iocage
```

and enable the activation of *iocage*

```yaml
freebsd_iocage_activate: true
```

* By default, sanity doesn't test the activation of *iocage*. You
  might want to enable it if you've already activated *iocage*

```yaml
freebsd_iocage_sanity_zfs_pool_active: true
```

* Optionally, update *defaults.json* if you've already activated
  *iocage*. For example,

```yaml
freebsd_iocage_defaults_update:
  allow_mount: 1
```

* By default, the *iocage* service is enabled, but not started because
  the command */usr/local/etc/rc.d/iocage start* is not
  idempotent. This means, the role will always report *changed* when
  you enable *freebsd_iocage_start*

```yaml
freebsd_iocage_enable: true
freebsd_iocage_start: false
```

* The other *iocage* service actions (restart and stop) are also not
  idempotent and are disabled by default
  
```yaml
freebsd_iocage_restart: false
freebsd_iocage_stop: false
```

* By default, the sanity is teasing the *iocage* service actions are
  mutually exclusive. The first enabled action in the order (start,
  restart, stop) is taken if you bypass this test
	
```yaml
freebsd_iocage_sanity_service: true
```

## Workflow

1) Change shell on the remote host to /bin/sh if necessary

```bash
shell> ansible host -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod admin -s /bin/sh'
```

2) Install the roles. The role *vbotka.freebsd_postinstall* is optional. You can use it to
   configure *sysctl.conf, loader.conf, fstab, environment, ZFS, ...*

```bash
shell> ansible-galaxy role install vbotka.freebsd_iocage
shell> ansible-galaxy role install vbotka.freebsd_postinstall
```

Install the collections if necessary. The collections *ansible.posix* and *ansible.utils* are
needed by the role *vbotka.freebsd_postinstall*

```bash
shell> ansible-galaxy collection install ansible.posix
shell> ansible-galaxy collection install ansible.utils
shell> ansible-galaxy collection install community.general
```

3) Fit variables to your needs.


4) Create playbook and inventory

```bash
shell> cat freebsd_iocage.yml
- hosts: host
  roles:
    - vbotka.freebsd_iocage
```

```bash
shell> cat hosts
[host]
<host-ip-or-fqdn>
[host:vars]
ansible_connection=ssh
ansible_user=admin
ansible_become=true
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python3.11
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure the iocage host

Check syntax of the play

```bash
shell> ansible-playbook freebsd-iocage.yml --syntax-check
```

Display variables

```bash
shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_debug -e freebsd_iocage_debug=true
```

Dry-run the package installation

```bash
ansible-playbook freebsd-iocage.yml -t freebsd_iocage_pkg -e freebsd_iocage_install=true -CD
```

Install packages

```bash
shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_pkg -e freebsd_iocage_install=true
```

Optionally, activate iocage

```bash
shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_activate -e freebsd_iocage_activate=true
```

Run sanity tests

```bash
shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_sanity
```

Optionally, configure iocage defaults

```bash
shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_conf
```

Dry-run the play and display the potential differences

```bash
shell> ansible-playbook freebsd_iocage.yml -CD
```

Run the playbook. The role is idempotent. If you're sure the configuration is correct you
can run the complete play

```bash
shell> ansible-playbook freebsd_iocage.yml
```

### Start iocage

By default, the *iocage* service is enabled, but not started because
the command */usr/local/etc/rc.d/iocage start* is not
idempotent. Keep this default and start the service by extra vars

```bash
shell> ansible-playbook freebsd_iocage.yml -e freebsd_iocage_start=true
```

The role will fail if any service action (start, restart, or stop) is
active and the service is disabled. You can selectively enable and
start the service

```bash
shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_rcconf -e freebsd_iocage_start=true -e freebsd_iocage_enable=true
```

### Restart iocage

* For example, enable *freebsd_iocage_restart* if you update *rc.conf*
  
```bash
shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_rcconf -e freebsd_iocage_restart=true
```

, or *defaults.json*

```bash

shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_rcconf,freebsd_iocage_conf -e freebsd_iocage_restart=true
```


### Disable iocage

* Stop the service first if you want to disable it

```bash
shell> ansible-playbook freebsd-iocage.yml -t freebsd_iocage_rcconf  -e freebsd_iocage_stop=true -e freebsd_iocage_enable=false
```


## Ansible lint

Use the configuration file *.ansible-lint.local* when running
*ansible-lint*. Some rules might be disabled and some warnings might
be ignored. See the notes in the configuration file.

```bash
shell> ansible-lint -c .ansible-lint.local
```


## References

- [FreeBSD handbook: Jails](https://docs.freebsd.org/en/books/handbook/jails/)
- [iocage - documentation](https://iocage.readthedocs.io/en/latest/)
- [GitHub: iocage](https://github.com/iocage/iocage)


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.info)
