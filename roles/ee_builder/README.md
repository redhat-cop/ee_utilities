# redhat_cop.ee_utilities.ee_builder

Ansible role to prep to use the setup an execution environment. This role invokes ansible builder and depends on either variables or files being provided to specify inclusions

## Requirements

ansible-builder
podman or docker

## Role Variables

Available variables are listed below, along with default values defined (see defaults/main.yml)

### Build Argument Defaults

|Variable Name|Default Value|Required|Description|Example|
|:---:|:---:|:---:|:---:|:---:|
|`builder_dir`|playbook_directory|no|The directory to store all build and context files|'/tmp'|
|`ee_builder_dir_clean`|true|no|Whether to delete the build dir when done.||
|`ee_container_runtime`|podman|no|container run time to use podman/docker.|podman|
|`ee_version`|1|no|What Execution Environment version to use||
|`galaxy_cli_opts`|-v|no|Options to apply when using ansible galaxy cli.||
|`ee_galaxy_keyring`||no|Options verify collection signatures during installation.||
|`ee_prune_images`|true|no|To enable or disable pruning the images after building.||
|`ee_stream`|upstream unless ee_base_registry_username is defined then downstream|no|What stream to pull images from either upstream or downstream.||
|`ee_update_base_images`|false|no|Whether to pull down images, this forces an update to avoid stale images.||
|`ee_builder_image`|registry.redhat.io/ansible-automation-platform-22/ansible-builder-rhel8:latest|no|Build arg specifies the image used for compiling type tasks.||
|`ee_base_registry_username`|ee_registry_username|no|Username to use when authenticating to base registries. If neither ee or base registry provided will be omitted.||
|`ee_base_registry_password`|ee_registry_password|no|Password to use when authenticating to base registries. If neither ee or base registry provided will be omitted.||
|`ee_create_ansible_config`|true|no|Whether or not to create the ansible config file for use in building an Execution Environment.||
|`ah_host`||no|Host to use for ansible config file.||
|`ah_token`||no|Token to use for ansible config file.||

### Execution environment list

This role takes a list of execution environments to describe a state.
It takes variables from the following sections the list variables section.

|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`ee_list`|`list`|yes|Data structure describing your Execution Environments Described below.|

#### List variables for Execution environment definition

|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`ee_name`||yes|Name of the ee image to create.|
|`ee_base_image`|"registry.redhat.io/ansible-automation-platform-22/ee-minimal-rhel8:latest"|no|Build arg specifies the base image for the execution environment to use.|
|`prepend`|list|no|Ansible Builder Additional commands may be specified in the additional_build_steps section, for execution before the main build steps (prepend).|
|`append`|list|no|Ansible Builder Additional commands may be specified in the additional_build_steps section, for execution after the main build steps (append).|
|`bindep`|list|no|The variable list to provide bindep requirements if using variables|
|`python`|list|no|The variable list to provide python requirements if using variables|
|`collections`|list|no|The variable list to provide collection galaxy requirements if using variables, in ansible galaxy list form|
|`roles`|list|no|The variable list to provide galaxy roles requirements if using variables, in ansible galaxy list form|

### Registry Step defaults

|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`ee_registry_username`||no|Username to use when authenticating to destination registries.|
|`ee_registry_password`||no|Password to use when authenticating to destination registries.|
|`ee_registry_dest`||no|Path or URL where image will be pushed.|
|`ee_image_push`|True|no|Control to choose whether to push image to registry or not.|
|`ee_auth_file`||no|Path to file containing authorization credentials to the remote registry.|
|`ee_executable`||no|Path to podman executable if it is not in the $PATH on the machine running podman.|
|`ee_ca_cert_dir`||no|Path to directory containing TLS certificates and keys to use.|
|`ee_validate_certs`||no|Require HTTPS and validate certificates when pulling or pushing. |
|`ee_sign_by`||no|Path to a key file to use to sign the image.|

## Example Playbook

The following playbook can be invoked in the following manner. This role is meant to build and push an execution Environment to an registry

```sh
ansible-playbook playbook.yml
```

```yaml
---
- name: Playbook to create custom EE
  hosts: localhost
  gather_facts: false
  collections:
    - redhat_cop.ee_utilities
  vars:
    ee_registry_dest: ahnosso.node
    ee_registry_username: admin
    ee_registry_password: secret123
    ee_list:
      - ee_name: custom_ee
        # base_image
        bindep:
          - python38-requests [platform:centos-8 platform:rhel-8]
          - python38-pyyaml [platform:centos-8 platform:rhel-8]
        python:
          - pytz  # for schedule_rrule lookup plugin
          - python-dateutil>=2.7.0  # schedule_rrule
          - awxkit  # For import and export modules
        collections:
          - name: awx.awx
          - name: redhat_cop.controller_configuration
          - name: redhat_cop.ah_configuration
        prepend:
          - RUN whoami
          - RUN cat /etc/os-release
        append:
          - RUN echo This is a post-install command!
  roles:
    - redhat_cop.ee_utilities.ee_builder
```

## License

MIT

## Author Information

Sean Sullivan
