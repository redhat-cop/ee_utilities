# Redhat Communities of Practice Execution Environment Utilities Collection

![pre-commit tests](https://github.com/redhat-cop/ee_utilities/actions/workflows/pre-commit.yml/badge.svg)
![Galaxy Release](https://github.com/redhat-cop/ee_utilities/workflows/galaxy-release/badge.svg)
<!-- Further CI badges go here as above -->

This ansible collection includes a number of roles which can be useful for managing Ansible Execution Environments. Using this collection, you'll be able to automate following tasks:

* prepare and maintain Ansible Execution Environments

## Redhat Communities of Practice Configuration Collections Suite

|Collection Name|Purpose|
|:---:|:---:|
|[Controller Configuration](https://galaxy.ansible.com/redhat_cop/controller_configuration)|Automation controller configuration|
|[Hub Configuration](https://galaxy.ansible.com/redhat_cop/ah_configuration)|Automation hub configuration|
|[EE Utilities](https://galaxy.ansible.com/redhat_cop/ee_utilities)|Execution Environment creation utilities|
|[AAP installation Utilities](https://galaxy.ansible.com/redhat_cop/aap_utilities)|Ansible Automation Platform Utilities|
|[AAP Configuration Template](https://github.com/redhat-cop/aap_configuration_template)|Configuration Template for this suite|

## Included content

Click the `Content` button to see the list of content included in this collection.

## Installing this collection

You can install the redhat_cop ee_utilities collection with the Ansible Galaxy CLI:

    ansible-galaxy collection install redhat_cop.ee_utilities

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:

<!-- markdownlint-disable MD046 -->
```yaml
---
collections:
  - name: redhat_cop.ee_utilities
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

See [LICENCE](https://www.gnu.org/licenses/gpl-3.0.txt) to see the full text.
