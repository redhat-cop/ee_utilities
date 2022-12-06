=====================================
Redhat_Cop.Ee_Utilities Release Notes
=====================================

.. contents:: Topics


v2.0.4
======

Minor Changes
-------------

- Removed ``ee_create_ansible_config`` in favour of ``ee_pull_collections_from_hub`` in ``ee_builder`` role.

v2.0.3
======

Bugfixes
--------

- Fixed issue where ansible.cfg file was not being consumed by the builder.

v2.0.2
======

Minor Changes
-------------

- added ability to output controller_configuration execution_environment config for created images.
- added ability to use base and builder images at several levels and updated README to be more accurate.
- added new options from ansible builder.
