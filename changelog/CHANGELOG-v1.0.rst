=======================================
vbotka.freebsd_iocage 1.0 Release Notes
=======================================

.. contents:: Topics


0.2.3
=====

Release Summary
---------------
Feature update.

Major Changes
-------------

Minor Changes
-------------
* Update README.
* Add block/rescue to tasks/pkg.yml
* Update tasks/sanity.yml; Report list actions_active
* Update all keywords environment with the variable freebsd_iocage_env


0.2.2
=====

Release Summary
---------------
Feature update.

Major Changes
-------------

* Add var freebsd_iocage_assert_quiet (default=true)
* Update tasks/sanity.yml
* Add var freebsd_iocage_data_fstab (default=false). Optionally create
  fstab entry by ansible.posix.mount
* Update tasks/data.yml
* Add var freebsd_iocage_clean (default=false). Optionally clean
  stopped jails.
* Add tasks/clean.yml


0.2.1
=====

Release Summary
---------------
Feature update.

Major Changes
-------------
* Add var freebsd_iocage_data_mount_enable (default=false)
* Add var freebsd_iocage_data_dir (default=mnt)
* Update tasks
* Update defaults and samples in vars.

Minor Changes
-------------
* Update README. Fix role link in galaxy.

Breaking Changes / Porting Guide
--------------------------------
* Rename:
  freebsd_iocage_zfs_pool to freebsd_iocage_pool
  freebsd_iocage_zfs_mount to freebsd_iocage_mount


0.2.0
=====

Release Summary
---------------
Feature update.

Major Changes
-------------

* Add tasks/data.yml to create and populate a dataset for mounting in
  a jail. Add var freebsd_iocage_data (default=false)

Minor Changes
-------------
* Update README.
* Update vars/\*.yml.samples
* Add files/firstboot.sh

Breaking Changes / Porting Guide
--------------------------------
* Variable -freebsd_iocage_zfs_ds renamed to freebsd_iocage_mount


0.1.1
=====

Release Summary
---------------
Maintenance update.

Major Changes
-------------

Minor Changes
-------------
* Split defaults/main.yml to defaults/main/\*.yml
* Tasks formatting improved.
* Add backup option for defaults.json
* Update tasks/debug.yml
* Add tasks/runner.yml; add var freebsd_iocage_runner (default=false)
* Add tasks/stat.yml; add var freebsd_iocage_stat (default=true)
* Add var freebsd_iocage_sysctl_update; Configure /etc/sysctl.conf
* Add sanity option freebsd_iocage_sanity_kernel_sctp (default=false)
* Update vars/\*.yml.sample
* Update README


0.1.0
=====

Release Summary
---------------

Initial commit to GitHub.

Major Changes
-------------

Minor Changes
-------------

Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------
