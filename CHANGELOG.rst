==============================
Ansible.Receptor Release Notes
==============================

.. contents:: Topics


v1.0.0
======

Major Changes
-------------

- prepare for release - updated some formatting as well as including changelogs + EE.yml file for certification

Minor Changes
-------------

- don't install receptorctl - upstream users will install via pip instead
- update galaxy.yml - added tags and added runtime file for ansible version support

Bugfixes
--------

- roles/setup - As well as preferring 'true' to 'yes', and marking commands that don't change anything as always unchanged.
