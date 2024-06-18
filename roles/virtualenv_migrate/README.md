# infra.ee_utilities.virtualenv_migrate

Use this role to create a list of python requirements from custom virtualenvs present in your AAP 1.2 cluster, after comparing those with requirements in Default Execution Environment.
This role is based on the `awx-manage` utility and needs an AAP1.2 tower node to gather requirements from and localhost to pull EE and compare those requirements.
This role sets the python requirements to `ee_python` variable, which can then be used by the ee_utilities role to create a new EE.

This role must be run against the tower AAP(1.2) host. As the same objects exist on multiple tower nodes in same cluster, ideally you can run this against one of those nodes and it will give sufficient results.

This role is supposed to work with the tower AAP(1.2) nodes only, which had the ability to create custom virtualenvs instead of execution environments.

This role should be run as root or become:true

Sample inventory file looks like this

```ini
[tower]
TOWER_HOST ansible_ssh_private_key=<>
```

## Requirements

podman on localhost

## Role variables

|Variable Name|Default Value|Required|Description|Example|
|:---:|:---:|:---:|:---:|:---:|
|`venv_migrate_default_ee_url`|`registry.redhat.io/ansible-automation-platform-24/ee-minimal-rhel9:latest`|no|"Registry link of the EE you want to compare requirements with"|`localhost/ee:latest`|
|`registry_username`|None|yes(for default EE value)|username to sign in to the registry|`admin`|
|`registry_password`|None|yes(for default EE value)|password to sign in to the registry|`pass`|
|`ee_collections`|None|No|List of collections to add to the execution environments. Must be in a requirements.yml galaxy format.|``|
|`venv_migrate_show_diff_with_default`|`False`|No|Include default venv with the list of virtual environments scanned.|``|
|`venv_migrate_ee_python_list`|None|No|This is an output variable, if you want to pass the requirements for ee_building|debug:msg="{{ venv_migrate_ee_python_list }}"|

## Example Playbook

```yaml
# playbook to gather requirements from custom virtualenvs
---
- name: Review custom virtualenvs and pull requirements
  hosts: tower
  become: true
  tasks:
    - name: Include venv role
      include_role:
        name: infra.ee_utilities.virtualenv_migrate
```

## Example Playbook Using both roles together

```yaml
# playbook to create EE's from an existing tower.
---
- name: Playbook to create custom EE
  hosts: tower
  gather_facts: false
  collections:
    - infra.ee_utilities
  vars:
    venv_migrate_default_ee_url: registry.redhat.io/ansible-automation-platform-24/ee-minimal-rhel9:latest
    ee_collections:
      - name: awx.awx
      - name: infra.controller_configuration
      - name: infra.ah_configuration
  tasks:
    - name: Include venv_migrate role
      include_role:
        name: infra.ee_utilities.virtualenv_migrate

    - name: ee_list
      ansible.builtin.debug:
        var: ee_list

- name: Build EEs with ee_builder role
  hosts: localhost
  vars:
    ee_registry_dest: hub
    ee_ah_host: hub
    ee_ah_token: f12ca3e34afcbfe2c81a563ad5446ae61cd7d530
    ee_validate_certs: false
    ee_list: "{{ hostvars[groups['tower'][0]]['ee_list'] }}"
    ee_image_push: false
  tasks:

    - name: Create EE
      include_role:
        name: infra.ee_utilities.ee_builder

    - name: Export python virtual enviroment list to file
      copy:
        content: "{{ ee_list | to_nice_yaml( width=50, explicit_start=True, explicit_end=True) }}"
        dest: venv_migrate_ee_python.yaml
...
```

## License

[GPLv3+](https://github.com/redhat-cop/ee_utilities#licensing)

## Author Information

Anshul Behl
