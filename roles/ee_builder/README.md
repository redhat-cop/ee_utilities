# redhat_cop.ee_utilities.ee_builder

Ansible role use to build execution environments. This role invokes ansible builder and depends on certain variables or files being provided.

## Requirements

ansible-builder
podman or docker

## Role Variables

Available variables are listed below, along with default values defined (see defaults/main.yml)

Order of preferences for images

1. ee_list images

    ```yaml
        ee_list:
          - name: custom_ee
            base_image: image_name
            builder_image: image_name
    ```

2. 'ee_base_image' and 'ee_builder_image' top level variables.

3. If none of the above are set, a default will be used.

   Downstream images from the redhat registery will be used if you provide a 'ee_base_registry_username'
   Otherwise it will default to the upstream images on quay.

    ```yaml
    __ee_stream_images:
      upstream:
        base_image: quay.io/ansible/ansible-runner:latest
        builder_image: quay.io/ansible/ansible-builder:latest
      downstream:
        base_image: registry.redhat.io/ansible-automation-platform-23/ee-minimal-rhel8:latest
        builder_image: registry.redhat.io/ansible-automation-platform-23/ansible-builder-rhel8:latest
    ```

Best practice is to use the default images, unless needing to pull from another repository.

### Build Argument Defaults

|Variable Name|Default Value|Required|Type|Description|Example|
|:---:|:---:|:---:|:---:|:---:|:---:|
|`builder_dir`|playbook_directory|no|str|The directory to store all build and context files.|'/tmp'|
|`ee_builder_dir_clean`|true|no|bool|Whether to delete the build dir when done.|true|
|`ee_container_runtime`|podman|no|str|container run time to use podman/docker.|podman|
|`ee_version`|1|no|int|What Execution Environment version to use.|2|
|`galaxy_cli_opts`|-v|no|str|Options to apply when using ansible galaxy cli.||
|`ee_galaxy_keyring`||no|str|Path to the keyring to verify collection signatures during installation.||
|`ee_galaxy_ignore_signature_status_code`||no|list|List of status codes to ignore while verifying collections.|-500|
|`galaxy_required_valid_signature_count`||no|int|Number of required valid collection signatures.|5|
|`ee_container_policy`||no|str|The container image validation policy to use with podman. Can be one of 'ignore_all', 'system','signature_required'.|ignore_all|
|`ee_verbosity`|0|no|int|Options Increase the output verbosity, can be from 0-3.||
|`ee_prune_images`|true|no|bool|To enable or disable pruning the images after building.||
|`ee_stream`|upstream unless ee_base_registry_username is defined then downstream|no|str|What stream to pull images from either upstream or downstream.||
|`ee_update_base_images`|false|no|bool|Whether to pull down images, this forces an update to avoid stale images.||
|`ee_create_controller_def`|false|no|bool|Whether to create controller definitions||
|`ee_base_image`|registry.redhat.io/ansible-automation-platform-23/ee-minimal-rhel8:latest|no|str|Build arg specifies parent image for the execution environment.||
|`ee_builder_image`|registry.redhat.io/ansible-automation-platform-23/ansible-builder-rhel8:latest|no|str|Build arg specifies the image used for compiling type tasks.||
|`ee_base_registry_username`|ee_registry_username|no|str|Username to use when authenticating to base registries. If neither ee or base registry provided will be omitted.||
|`ee_base_registry_password`|ee_registry_password|no|str|Password to use when authenticating to base registries. If neither ee or base registry provided will be omitted.||
|`ee_pull_collections_from_hub`|true|no|bool|Whether or not to pull collections from a specific hub for use in building an Execution Environment.||
|`ee_ah_host`|`ah_host`|no|str|Host to use for ansible config file. Alternative default is to use variable from infra.ah_configuration. Required if `ee_pull_collections_from_hub` is `True`.||
|`ee_ah_token`|`ah_token`|no|str|Token to use for ansible config file. Alternative default is to use variable from infra.ah_configuration. Required if `ee_pull_collections_from_hub` is `True`.||

### Execution environment list

This role takes a list of execution environments to describe a state.
It takes variables from the following sections the list variables section.

|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`ee_list`|`list`|yes|Data structure describing your Execution Environments Described below.|

#### List variables for Execution environment definition

|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`name`||yes|Name of the ee image to create. Only the name goes here, the namespace goes in the ee_registry_dest variable|
|`tag`||no|Tag to use when pushing the image.|
|`base_image`|"registry.redhat.io/ansible-automation-platform-23/ee-minimal-rhel8:latest"|no|Build arg specifies the base image for the execution environment to use.|
|`builder_image`|registry.redhat.io/ansible-automation-platform-23/ansible-builder-rhel8:latest|no|str|Build arg specifies the image used for compiling type tasks.||
|`prepend`|list|no|Ansible Builder Additional commands may be specified in the additional_build_steps section, for execution before the main build steps (prepend).|
|`append`|list|no|Ansible Builder Additional commands may be specified in the additional_build_steps section, for execution after the main build steps (append).|
|`bindep`|list|no|The variable list to provide bindep requirements if using variables|
|`python`|list|no|The variable list to provide python requirements if using variables|
|`collections`|list|no|The variable list to provide collection galaxy requirements if using variables, in ansible galaxy list form|
|`roles`|list|no|The variable list to provide galaxy roles requirements if using variables, in ansible galaxy list form|

#### Additional List variables for Execution environment definition for Controller configuration

These variables are only use in creating the Execution Environment 'controller_execution_environments' definition that is useable wtih the infra.controller_configuration role to push definitions to the Automation controller.

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---:|
|`alt_name`|`name`|no|str|Alternate name of the ee image to create.|
|`description`|""|no|str|Description to use for the execution environment.|
|`organization`|""|no|str|The organization the execution environment belongs to.|
|`pull`|"missing"|no|choice("always", "missing", "never")|Determine image pull behavior|
|`ee_reg_credential`|""|no|str|Name of the credential to use for the execution environment.|

### Registry Step defaults

|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`ee_registry_username`||no|Username to use when authenticating to destination registries.|
|`ee_registry_password`||no|Password to use when authenticating to destination registries.|
|`ee_registry_dest`||no|Path or URL where image will be pushed. Namespaces for containers go here. Examples: registry.redhat.io, registry.redhat.io/rh-custom |
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
      - name: custom_ee
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
            type: url
            source: https://galaxy.ansible.com/download/awx-awx-21.11.0.tar.gz
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

This is an example for building using automated pipelines like Gitlab or Azure Devops where the build container and other dependencies used for building the final artifact are destroyed after the pipeline is finished
```yaml
---
- name: Playbook to create custom EE
  hosts: localhost
  gather_facts: false
  collections:
    - infra.ee_utilities
    # One of these two may be required in certain environments
    # - containers.podman
    # - community.docker
  vars:
    ee_base_registry_username: admin
    ee_base_registry_password: secret123
    # As stated in ee_registry_dest's description, if you want to namespace an image you put the namespace in the ee_registry_dest variable like so instead of in the name variable
    ee_registry_dest: ahnosso.node/custom-images-for-prod
    ee_registry_username: admin
    ee_registry_password: secret123
    # in this example we are assuming that we are pulling content and pushing the final artifact to the same location
    ee_ah_host: ahnosso.node
    ee_ah_token: iamatoken
    # ee-builder_dir_clean is used because depending on the environment permissions errors can be thrown when attempting to clean up. It is also unnecessary if the entire environment is going to be destroyed at the end anyway.
    #
    ee_builder_dir_clean: false
    #  builder_dir is set to the relative path "." because it tells ansible-builder to always use the temporary folder created by the pipeline. This may not be necessary depending on the envirnment but the temporary directories created by the pipeline for building the final artifacts can vary in location
    builder_dir: "."
    ee_list:
        # To reiterate, only the name variable goes here, not the namespace, that is placed in ee_registry_dest, please refer to ee_registry_dest's description for more details
      - name: custom_ee
        # Using the latest tag is best practice and should be replaced with a tested version of the container. However latest can be a good starting point to figure out which container works, then replacing latest with the version number for the tested latest container.
        base_image: registry.redhat.io/ansible-automation-platform/ee-minimal-rhel8:latest
        builder_image: registry.redhat.io/ansible-automation-platform-23/ansible-builder-rhel8:latest
        bindep:
          - python38-requests [platform:centos-8 platform:rhel-8]
          - python38-pyyaml [platform:centos-8 platform:rhel-8]
        python:
          - pytz  # for schedule_rrule lookup plugin
          - python-dateutil>=2.7.0  # schedule_rrule
          - awxkit  # For import and export modules
        collections:
          - name: awx.awx
            type: url
            source: https://galaxy.ansible.com/download/awx-awx-21.11.0.tar.gz
          - name: redhat_cop.controller_configuration
          - name: redhat_cop.ah_configuration
        prepend:
          - RUN whoami
          - RUN cat /etc/os-release
        append:
          - RUN echo This is a post-install command!
  # This pre-task section is provided because older environment or environments like build pipelines may not have ansible-builder pre-installed. This is a good place to install other dependencies that need to be in the build pipeline its self not in the final artifact.
#  pre_tasks:
#    - name: install ansible-builder
#      ansible.builtin.pip:
#        name: ansible-builder
#        executable: pip3.9
#      tags: always
  roles:
    - infra.ee_utilities.ee_builder
```

## License

[GPLv3+](https://github.com/redhat-cop/ee_utilities#licensing)

## Author Information

Sean Sullivan
Jonathan Bouligny
