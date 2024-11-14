=======================================
vbotka.freebsd_iocage 1.0 Release Notes
=======================================

.. contents:: Topics


0.1.2
=====

Release Summary
---------------
Maintenance update.

Major Changes
-------------

Minor Changes
-------------
Update README.


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
