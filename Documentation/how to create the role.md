Creating an Ansible role to configure UEFI boot by adding a picture and text is a complex task. UEFI boot configurations are typically managed at a low level on a system, and Ansible is generally used for higher-level configuration management. However, I can guide you through the creation of a basic Ansible role structure that might be used for this purpose. 

Please note, this role will be quite basic and might need significant customization based on your specific environment and the boot manager you are using (e.g., GRUB2 for most Linux distributions).

1. **Role Directory Structure**:
   ```
   uefi_boot_config/
   ├── defaults
   │   └── main.yml
   ├── handlers
   │   └── main.yml
   ├── tasks
   │   └── main.yml
   ├── templates
   │   └── grub.cfg.j2
   └── vars
       └── main.yml
   ```

2. **Defaults (`defaults/main.yml`)**: 
   Define default variables. These can be overridden in your playbook.
   ```yaml
   uefi_boot_image: /path/to/your/image.tga
   uefi_boot_text: "Your Custom Boot Text"
   ```

3. **Tasks (`tasks/main.yml`)**:
   Outline the tasks to configure UEFI boot.
   ```yaml
   - name: Check if GRUB config exists
     ansible.builtin.stat:
       path: /etc/default/grub
     register: grub_config

   - name: Update GRUB config
     ansible.builtin.template:
       src: grub.cfg.j2
       dest: /etc/default/grub
     when: grub_config.stat.exists

   - name: Update GRUB
     ansible.builtin.shell:
       cmd: grub2-mkconfig -o /boot/grub2/grub.cfg
     when: grub_config.stat.exists
   ```

4. **Template (`templates/grub.cfg.j2`)**:
   Create a Jinja2 template for the GRUB configuration. You'll need to customize this based on the specific settings you want to change.
   ```jinja2
   GRUB_TIMEOUT=5
   GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo AlmaLinux`
   GRUB_DEFAULT=0
   GRUB_HIDDEN_TIMEOUT=0
   GRUB_HIDDEN_TIMEOUT_QUIET=true
   GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
   GRUB_CMDLINE_LINUX=""

   # Set UEFI boot image and text
   GRUB_BACKGROUND="{{ uefi_boot_image }}"
   # Add any additional configuration here
   ```

5. **Variables (`vars/main.yml`)**:
   Here, you can define any other variables that are not supposed to be overridden.

6. **Handlers (`handlers/main.yml`)**:
   You might not need handlers for this role unless you have services to restart or other triggered actions.

Remember, this is a basic template and needs to be customized to your specific requirements. Modifying boot loader configurations can be risky; incorrect configurations might prevent your system from booting. Always test changes in a controlled environment before applying them to production systems.

To use this role, you'd include it in your Ansible playbook and provide the necessary variables (like the path to the UEFI boot image and any custom text you want to display). Ensure you have the correct permissions and that Ansible can execute commands that may require sudo privileges.