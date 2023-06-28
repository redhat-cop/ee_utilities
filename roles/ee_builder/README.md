# infra.ee_utilities.ee_builder

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
    ```

2. 'ee_base_image' top level variables.

3. If none of the above are set, a default will be used.

   Downstream images from the redhat registery will be used if you provide a 'ee_base_registry_username'
   Otherwise it will default to the upstream images on quay. These are only used if no base is specificed.

    ```yaml
      upstream:
        base_image: quay.io/ansible/ansible-runner:latest
      downstream:
        base_image: registry.redhat.io/ansible-automation-platform-24/ee-minimal-rhel9:latest
    ```

Best practice is to use the default images, unless needing to pull from another repository.

### Build Argument Defaults

|Variable Name|Default Value|Required|Type|Description|Example|
|:---:|:---:|:---:|:---:|:---:|:---:|
|`ee_builder_dir`|playbook_directory|no|str|The directory to store all build and context files.|'/tmp'|
|`ee_builder_dir_clean`|true|no|bool|Whether to delete the build dir when done.|true|
|`ee_container_runtime`|podman|no|str|container run time to use podman/docker.|podman|
|`ee_version`|3|no|int|What Execution Environment version to use.|2|
|`ee_galaxy_keyring`||no|str|Path to the keyring to verify collection signatures during installation.||
|`ee_galaxy_ignore_signature_status_code`||no|list|List of status codes to ignore while verifying collections.|-500|
|`galaxy_required_valid_signature_count`||no|int|Number of required valid collection signatures.|5|
|`ee_container_policy`||no|str|The container image validation policy to use with podman. Can be one of 'ignore_all', 'system','signature_required'.|ignore_all|
|`ee_verbosity`|0|no|int|Options Increase the output verbosity, can be from 0-3.||
|`ee_prune_images`|true|no|bool|To enable or disable pruning the images after building.||
|`ee_stream`|upstream unless ee_base_registry_username is defined then downstream|no|str|What stream to pull images from either upstream or downstream.||
|`ee_update_base_images`|false|no|bool|Whether to pull down images, this forces an update to avoid stale images.||
|`ee_base_image`|registry.redhat.io/ansible-automation-platform-24/ee-minimal-rhel9:latest|no|str|Build arg specifies parent image for the execution environment. Use the images option to override this for an individual list item.||
|`ee_base_registry_username`|ee_registry_username|no|str|Username to use when authenticating to base registries. If neither ee or base registry provided will be omitted.||
|`ee_base_registry_password`|ee_registry_password|no|str|Password to use when authenticating to base registries. If neither ee or base registry provided will be omitted.||
|`ee_pull_collections_from_hub`|true|no|bool|Whether or not to pull collections from a specific hub for use in building an Execution Environment. This will create entries that adds the ansible.cfg file into the EE.||
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
|`dependencies`|dict|no|This section allows you to describe any dependencies that will need to be installed into the final image. Reference [builder dependencies documentation](https://ansible.readthedocs.io/projects/builder/en/stable/definition/#dependencies), examples and our examples for its structure.|
|`build_steps`|dict|no|This section enables you to specify custom build commands for any build phase. Reference [builder build_steps documentation](https://ansible.readthedocs.io/projects/builder/en/stable/definition/#additional-build-steps), examples and our examples for its structure.|
|`build_files`|dict|no|This section allows you to add any file to the build context directory. Reference [builder build_files documentation](https://ansible.readthedocs.io/projects/builder/en/stable/definition/#additional-build-files), examples and our examples for its structure.|
|`images`|dict|no|This section is a dictionary that is used to define the base image to be used. Reference [builder images documentation](https://ansible.readthedocs.io/projects/builder/en/stable/definition/#images), examples and our examples for its structure. This will override 'ee_base_image'.|
|`options`|dict|no|This section is a dictionary that contains keywords/options that can affect builder runtime functionality. Reference [builder options documentation](https://ansible.readthedocs.io/projects/builder/en/stable/definition/#options), examples and our examples for its structure.|

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
    - infra.ee_utilities
  vars:
    # For controller configuration definition
    ee_builder_dir_clean: false
    ee_builder_dir: "."
    ee_update_base_images: false
    ee_reg_credential: Automation Hub Container Registry
    ee_base_image: registry.redhat.io/ansible-automation-platform-24/ee-minimal-rhel9:latest
    ee_pull_collections_from_hub: true
    ah_host: hub.nas
    ah_token: ec28091dfebd9fb4c7ddc59d34cddb35350b71cb
    ee_registry_dest: ahnosso.node
    ee_registry_username: admin
    ee_registry_password: secret123
    ee_verbosity: 1
    ee_list:
      - name: custom_ee
        alt_name: Custom EE
        tag: 1-11-21-2
        dependencies:
          system:
          - python-requests
          - python-pyyaml
          python:
            - pytz  # for schedule_rrule lookup plugin
            - python-dateutil>=2.7.0  # schedule_rrule
            - awxkit  # For import and export modules
          galaxy:
            collections:
              - name: awx.awx
                version: 22.4.0
              - infra.controller_configuration
              - ansible.controller
              - infra.ah_configuration
        build_steps:
          prepend_final:
            - RUN whoami
            - RUN cat /etc/os-release
          append_final:
            - RUN echo This is a post-install command!
  roles:
    - infra.ee_utilities.ee_builder
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
    ee_base_image: registry.redhat.io/ansible-automation-platform-24/ee-minimal-rhel9:latest
    # As stated in ee_registry_dest's description, if you want to namespace an image you put the namespace in the ee_registry_dest variable like so instead of in the name variable
    ee_registry_dest: ahnosso.node/custom-images-for-prod
    ee_registry_username: admin
    ee_registry_password: secret123
    # in this example we are assuming that we are pulling content and pushing the final artifact to the same location
    ee_ah_host: ahnosso.node
    ee_ah_token: iamatoken
    # ee_builder_dir_clean is used because depending on the environment permissions errors can be thrown when attempting to clean up. It is also unnecessary if the entire environment is going to be destroyed at the end anyway.
    #
    ee_builder_dir_clean: false
    #  ee_builder_dir is set to the relative path "." because it tells ansible-builder to always use the temporary folder created by the pipeline. This may not be necessary depending on the envirnment but the temporary directories created by the pipeline for building the final artifacts can vary in location
    ee_builder_dir: "."
    ee_list:
        # To reiterate, only the name variable goes here, not the namespace, that is placed in ee_registry_dest, please refer to ee_registry_dest's description for more details
      - name: custom_ee
        # Using the latest tag is best practice and should be replaced with a tested version of the container. However latest can be a good starting point to figure out which container works, then replacing latest with the version number for the tested latest container.
        images:
          base_image:
            name: registry.redhat.io/ansible-automation-platform-24/ee-minimal-rhel9:latest
        dependencies:
          ansible_core:
            package_pip: ansible-core==2.15
          ansible_runner:
            package_pip: ansible-runner
          system:
            - python-requests
            - python-pyyaml
          python:
            - pytz  # for schedule_rrule lookup plugin
            - python-dateutil>=2.7.0  # schedule_rrule
            - awxkit  # For import and export modules
          galaxy:
            collections:
              - name: awx.awx
                version: 22.4.0
              - infra.controller_configuration
              - ansible.controller
              - infra.ah_configuration
        build_steps:
          prepend_final:
            - RUN whoami
            - RUN cat /etc/os-release
          append_final:
            - RUN echo This is a post-install command!
      # This overwrites the above base image.
      - name: custom_suported
        alt_name: Custom EE2
        ee_base_image: registry.redhat.io/ansible-automation-platform-24/ee-supported-rhel9:latest
        dependencies:
          galaxy:
            collections:
              - community.aws
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

Sean Sullivan and Jonathan Bouligny
