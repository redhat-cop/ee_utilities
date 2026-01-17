=================================
infra.ee\_utilities Release Notes
=================================

.. contents:: Topics

v4.2.2
======

Bugfixes
--------

- allow for providing the extra-build-cli-args option to ansible-builder

v4.2.0
======

Minor Changes
-------------

- Added support for building and pushing EE images with multiple tags.

Bugfixes
--------

- this should fix the ci action to build and publish an EE with all the collections

v4.1.2
======

Bugfixes
--------

- allow for providing the squash option to ansible-builder
- make ee_builder_dir work with multiple EEs defined in ee_list

v4.0.4
======

Bugfixes
--------

- Allow ee_ah_token to be blank for anonymous access

v4.0.1
======

Bugfixes
--------

- Fixes issue where ee_builder role concatenates the URL and token on ansible.cfg

v4.0.0
======

Minor Changes
-------------

- adds an ee_aap_version var to allow for pushing collections to either AAP 2.4 or 2.5
- updated 'ee_ah_host' and 'ee_ah_token' to also take aap_hostname and aap_token as defaults, updated readme to reflect new changes.

Breaking Changes / Porting Guide
--------------------------------

- update execution_environment.yml to execution-environment.yaml to be in line with the ansible-builder default. Only affects files generated. Any user-defined automation depending on this file will need to be updated.

Bugfixes
--------

- Fixed an issue where a tempfile was not created locally, but rather on the remote machine
- Fixed an issue where depending on your ee base image variable location, it might not pull the correct image.
- Fixed issue where the base image name was not able to be set for individual ee
- found some typoes in readme and corrected them.

v3.1.3
======

Bugfixes
--------

- Added an option of build_files to the ee definition. This will allow for files/folders to be copied to the working directory to be used for build files.
- Fixed an issue when ee_pull_collections_from_hub was set to true, and build_steps were not defined, an error was raised.

v3.1.2
======

Bugfixes
--------

- removed containers.podman dependency. It made it so you could not sync this collection to automation hub.

v3.1.1
======

v3.1.0
======

Minor Changes
-------------

- ansible.cfg removed from root and galaxy.yml added to enable install from source

v3.0.0
======

Breaking Changes / Porting Guide
--------------------------------

- Check the ee_builder documentation for updated vars, All dependency files moved to the dependency variable. Other options moved to their respective places as well.
- Updated the venv role to use v3 format, updated the documentation on this role as well.
- builder role rewritten from ground up to match the v3 ansible-builder structure. This has simplified many of the previous variables, allowing for more customization and allows for adaptability of future updates without code changes.

v2.0.8
======

Minor Changes
-------------

- Added changed_when false to built in commands that gather information.

v2.0.7
======

Minor Changes
-------------

- Added changed_when false to built in commands that gather information.

v2.0.6
======

v2.0.5
======

Breaking Changes / Porting Guide
--------------------------------

- renaming ee_name to name in the ee_list variable

Bugfixes
--------

- bug not removing ee_controller.yaml file
- bug when multiple EEs in ee_list would fail when ee_builder_dir_clean was true

v2.0.4
======

Minor Changes
-------------

- Removed ee_create_ansible_config in favour of ee_pull_collections_from_hub in ee_builder role.

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
