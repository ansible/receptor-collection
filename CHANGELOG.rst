==============================
Ansible.Receptor Release Notes
==============================

.. contents:: Topics

v2.0.7
======

Minor Changes
-------------

- Install more podman dependent packages to adopt RHEL 10

Bugfixes
--------

- Update CHANGELOG.rst

v2.0.6
======

Minor Changes
-------------

- Update collection to support ansible-core 2.19

Bugfixes
--------

- Fix the when condition on Install receptor packages task

v2.0.5
======

Minor Changes
-------------

- Updates for certified collection validation

v2.0.4
======

Minor Changes
-------------

- Bump python and pip version to 3.11
- Fix for check mode failure when no TLS files exist

v2.0.3
======

Minor Changes
-------------

- Add receptor_user/receptor_group variables
- Support old-style host plus port as well as new-style address strings

v2.0.2
======

Minor Changes
-------------

- Add molecule testing
- Do not run python packages related tasks if no additional_python_packages

v2.0.1
======

Bugfixes
--------

- Fix rendering of README table

v2.0.0
======

Major Changes
-------------

- Add debian to setup and podman roles
- Add handlers to restart or reload receptor
- Breakout config creation into role

Minor Changes
-------------

- Add readme files to roles
- Add receptor_service_name to setup-service.yml
- Check python version

Breaking Changes / Porting Guide
--------------------------------

- Use release install method by default instead of package

Bugfixes
--------

- Fix incorrect default value for receptor_install_method in documentation

v1.1.1
======

Minor Changes
-------------

- Removed dependencies section from execution-environment.yml

Bugfixes
--------

- Fix correct indentation and line breaks for receptor_peers

v1.1.0
======

Minor Changes
-------------

- Include a role for setting up podman

v1.0.0
======

Major Changes
-------------

- Prepare for release - updated some formatting as well as including changelogs + EE.yml file for certification

Minor Changes
-------------

- Don't install receptorctl - upstream users will install via pip instead
- Update galaxy.yml - added tags and added runtime file for ansible version support

Bugfixes
--------

- roles/setup - As well as preferring 'true' to 'yes', and marking commands that don't change anything as always unchanged.
