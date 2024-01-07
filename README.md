# Ansible Role: UEFI Boot Configuration

## Table of Contents
1. [Overview](#overview)
2. [Requirements](#requirements)
3. [Role Structure](#role-structure)
4. [Default Variables](#default-variables)
5. [Usage](#usage)
6. [Testing](#testing)
7. [Contributing](#contributing)
8. [License](#license)
9. [Author](#author)

## Overview
This Ansible role is designed for configuring UEFI boot settings on various systems. It provides a flexible and automated approach to manage UEFI boot options, ensuring consistency and reliability in boot configurations across your infrastructure.

## Requirements
- Ansible 2.x or higher.
- Appropriate privileges to manage UEFI settings on target machines.

## Role Structure
- `.github`: Contains GitHub-specific configurations and workflows.
- `.yamllint`: Yamllint configuration for ensuring YAML best practices.
- `Documentation`: Additional documentation for the role.
- `LICENSE`: The license file for the project.
- `defaults`: Default variables for the role.
- `files`: Static files used by the role.
- `handlers`: Handlers used for triggering actions based on task results.
- `molecule`: Molecule tests for testing the role's functionality.
- `tasks`: Main list of tasks to be executed by the role.
- `templates`: Jinja2 templates used for generating dynamic configurations.
- `vars`: Other variables for the role.
- `ysgd.pre-commit-config.yaml-old`: Legacy pre-commit configuration (if relevant).

## Default Variables
The role includes several default variables which can be overridden in your playbook or inventory file. These variables are located in the `defaults` directory.

- `grub_timeout`: The GRUB boot menu timeout. Default is `'5'`.
- `grub_distributor`: The GRUB distributor ID. Default is determined by the output of `$(sed 's, release .*$,,g' /etc/system-release)`.
- `grub_default`: Default boot entry. Default is `'saved'`.
- `grub_disable_submenu`: Whether to disable the GRUB submenu. Default is `'true'`.
- `grub_terminal_output`: Output terminal for GRUB. Default is `console`.
- `grub_cmdline_linux`: Additional kernel parameters. Default includes various parameters like `rd.driver.blacklist=nouveau` and others.
- `grub_disable_recovery`: Whether to disable recovery mode. Default is `'true'`.
- `grub_enable_blscfg`: Whether to enable the Boot Loader Specification (BLS). Default is `'true'`.
- `grub_boot_image`: Path to custom boot image. Default is `/boot/grub/boot_image.png`.

To override a variable, specify it in your playbook like this:

```yaml
vars:
  grub_timeout: "10"
```

## Usage
1. Install the role via Ansible Galaxy or by cloning this repository.
2. Customize your role variables located in the `defaults` and `vars` directories.
3. Include the role in your playbook.

Example playbook:
```yaml
- hosts: all
  roles:
    - ansible-role-uefi-boot-config
  vars:
    grub_timeout: "10"
```

## Testing
Utilize the provided Molecule tests in the `molecule` directory to test the role across different environments and scenarios.

## Contributing
Contributions are welcome. For guidelines on how to contribute to this project, please read the contribution documentation located at [Documentation/contribute.md](Documentation/contribute.md)

## License
This project is licensed under the GPL-3.0 License. See the `LICENSE` file for more details.

## Author
**Johan Sörell**

- GitHub: [J-SirL](https://github.com/J-SirL)
