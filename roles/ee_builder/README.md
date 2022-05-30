# redhat_cop.ee_utilities.ee_builder

Ansible role to prep to use the setup an execution environment. This role invokes ansible builder and depends on either variables or files being provided to specify inclusions

## Requirements

ansible-builder
podman or docker

## Role Variables

Available variables are listed below, along with default values defined (see defaults/main.yml)

### Build Arugment Defaults
|Variable Name|Default Value|Required|Description|Example|
|:---:|:---:|:---:|:---:|:---:|
|`builder_dir`|"playbook_directory"|no|The directory to store all build and context files|'/tmp'|
|`ee_container_runtime`|podman|no|container run time to use podman/docker.|podman|
|`ee_version`|1|no|What Execution Environment version to use||
|`galaxy_cli_opts`|"-v"|no|Options to apply when using ansible galaxy cli.||
|`ee_base_image`|""|no|Build arg specifies the parent image for the execution environment.||
|`builder_image`|""|no|Build arg specifies the image used for compiling type tasks.||

### Requirements File defaults
|Variable Name|Default Value|Required|Description|Example|
|:---:|:---:|:---:|:---:|:---:|
|`bindep_file`|python_req|no|The file to provide bindep requirements if using files|'/tmp'|
|`python_requirements_file`|bindep|no|The file to provide python requirements if using files|podman|
|`galaxy_requirements_file`|requirements.yml|no|The file to provide galaxy requirements if using files||
|`ee_jinja_templates_create`|True|no|To skip using the files, using requirement defaults below, and to template out all requirements files||

### Requirements Variable defaults
|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`ee_bindep`|list|no|The variable list to provide bindep requirements if using variables|
|`ee_python`|list|no|The variable list to provide python requirements if using variables|
|`ee_collections`|list|no|The variable list to provide galaxy requirements if using variables, in ansible galaxy list form|
|`ee_roles`|list|no|The variable list to provide galaxy requirements if using variables, in ansible galaxy list form|

### Build Step defaults
|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`ee_name`||no|Name of the ee image to create.|
|`ee_prepend`|list|no|Ansible Builder Additional commands may be specified in the additional_build_steps section, for execution before the main build steps (prepend).|
|`ee_append`|list|no|Ansible Builder Additional commands may be specified in the additional_build_steps section, for execution after the main build steps (append).|

### Registry Step defaults
|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`ee_registry_username`||no|username to use when authenticating to remote registries.|
|`ee_registry_password`||no|Password to use when authenticating to remote registries.|
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
$ ansible-playbook playbook.yml
```

```yaml
---
- name: Playbook to configure ansible controller organizations
  hosts: localhost
  gather_facts: false
  collections:
    - redhat_cop.ee_utilities
    - containers.podman
  vars:
    # builder_dir: /tmp
    ee_registry_dest: quay.io/your_place_here/ee_tools
    ee_name: ee_tools
    ee_bindep:
      - docker
    ee_python:
      - ansible-lint
      - ansible-builder
    ee_collections:
      - name: awx.awx
      - name: redhat_cop.tower_utilities
        version: 0.3.2-develh
      - name: containers.podman
      - name: community.docker
    ee_append:
      - VOLUME /var/lib/docker
  roles:
    - redhat_cop.ee_utilities.ee_builder
```

## License

MIT

## Author Information

Sean Sullivan
