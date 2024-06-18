# Redhat Communities of Practice Execution Environment Utilities Collection

[![pre-commit tests](https://github.com/redhat-cop/ee_utilities/actions/workflows/pre-commit.yml/badge.svg)](https://github.com/redhat-cop/ee_utilities/actions/workflows/pre-commit.yml)
[![Run Test of EE utilities](https://github.com/redhat-cop/ee_utilities/actions/workflows/ci_testing.yaml/badge.svg)](https://github.com/redhat-cop/ee_utilities/actions/workflows/ci_testing.yaml)
[![Galaxy Release](https://github.com/redhat-cop/ee_utilities/actions/workflows/release.yml/badge.svg)](https://github.com/redhat-cop/ee_utilities/actions/workflows/release.yml)

This ansible collection includes a number of roles which can be useful for managing Ansible Execution Environments. Using this collection, you'll be able to automate following tasks:

* prepare and maintain Ansible Execution Environments

## Getting Help

We are on the Ansible Forums and Matrix, if you want to discuss something, ask for help, or participate in the community, please use the #infra-config-as-code tag on the fourm, or post to the chat in Matrix.

[Ansible Forums](https://forum.ansible.com/tag/infra-config-as-code)

[Matrix Chat Room](https://matrix.to/#/#aap_config_as_code:ansible.com)

## Requirements

The containers.podman collection MUST be installed in order for this collection to work.
In addition the podman executable is required for the containers.podman collection.
It is not listed as a dependency because of issues with syncing the validated content repo from console.redhat.com.

## Links to Ansible Automation Platform Collections

|                                      Collection Name                                         |                 Purpose                  |
|:--------------------------------------------------------------------------------------------:|:----------------------------------------:|
| [awx.awx/Ansible.controller repo](https://github.com/ansible/awx/tree/devel/awx_collection) |   Automation controller modules          |
|        [Ansible Hub Configuration](https://github.com/ansible/automation_hub_collection)     |       Automation hub configuration       |

## Links to other Validated Configuration Collections for Ansible Automation Platform

|                                      Collection Name                                       |                 Purpose                  |
|:------------------------------------------------------------------------------------------:|:----------------------------------------:|
| [Controller Configuration](https://github.com/redhat-cop/controller_configuration) |   Automation controller configuration    |
|             [EE Utilities](https://github.com/redhat-cop/ee_utilities)             | Execution Environment creation utilities |
|     [AAP installation Utilities](https://github.com/redhat-cop/aap_utilities)      |  Ansible Automation Platform Utilities   |
|   [AAP Configuration Template](https://github.com/redhat-cop/aap_configuration_template)   |  Configuration Template for this suite   |

## Included content

Click the `Content` button to see the list of content included in this collection.

## Installing this collection

You can install the infra.ee_utilities collection with the Ansible Galaxy CLI:

    ansible-galaxy collection install infra.ee_utilities

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:

<!-- markdownlint-disable MD046 -->
```yaml
---
collections:
  - name: infra.ee_utilities
    # If you need a specific version of the collection, you can specify like this:
    # version: ...
```

## Using this collection

### See Also

* [Ansible Using collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html) for more details.

## Release and Upgrade Notes

## Releasing, Versioning and Deprecation

This collection follows [Semantic Versioning](https://semver.org/). More details on versioning can be found [in the Ansible docs](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html#collection-versions).

We plan to regularly release new minor or bugfix versions once new features or bugfixes have been implemented.

Releasing the current major version happens from the `devel` branch.

## Roadmap

## Contributing to this collection

We welcome community contributions to this collection. If you find problems, please open an issue or create a PR against the [Tower Utilities collection repository](https://github.com/redhat-cop/ee_utilities).
More information about contributing can be found in our [Contribution Guidelines.](https://github.com/redhat-cop/ee_utilities/blob/devel/.github/CONTRIBUTING.md)

## Licensing

GNU General Public License v3.0 or later.

See [LICENCE](LICENSE) to see the full text.
