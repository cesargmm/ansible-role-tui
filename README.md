# Ansible Role - TUI

```text
                         ✨
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢠⣖⡒⠢⢄⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀✨⠀⠀⠀⠀⠀⢸⣿⣇⠀⠀⠈⢦⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣰⣿⣿⠀⠀⠀⢠⣿⠉⠒⠢⠤⣀⠀⠀⠀⠀⠀⠀⠀⠀✨
⠀⠀⠀⠀⠀⠀⠀⠀⢀⠔⠊⢀⣳⣤⣤⣶⣾⣿⣿⣦⣤⣤⣤⣾⣿⣦⠀⠀⠀⠀⠀⠀
⠀⠀⠀    ⠀⢸⣿⡷⣾⡟⣟⢭⠙⠛⢱⣲⢹⢻⣿⣿⣿⡿⣿⣿⡀⠀⠀⠀⠀⠀
⠀⠀⠀✨⠀⠀⠀⣿⣿⢠⢣⣯⡛⠀⠄⠈⠋⢀⡆⣿⡏⠁⠀⢸⣿⡇⠀⠀⠀⠀⠀
⠀⠀⠀  ⠀⠀⠀⠛⠻⡚⠺⠿⠟⢓⣢⡀⠚⠓⠚⠛⠃⢂⠄⠈⠉⠁⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠲⣀⠤⢲⠻⣐⡸⣀⢹⣶⣤⣀⠖⠁⠀⠀⠀⠀✨⠀⠀⠀⠀
⠀⠀⠀⠀⠀⢀⠤⠒⠒⠤⣀⡠⠔⢇⠀⣀⡸⣆⠇⢹⣿⣿⣷⠢⠤⣀⠀⠀⠀⠀⠀⠀
⠀✨⠀⠀⠀⠉⡆⠀⠀⠀⢀⡠⠋⠓⠁⠀⠀⠀⢾⣿⣿⣿⣧⠀⠀⣑⡢⠀⠀⠀⠀
⠀  ⠀⠀⠀⠀⠈⠢⠤⠊⠁⠀⠀⠀✨⠀⠀⠀⠉⠁⠈⠻⠧⠊⠀⠀⠀⠀⠀⠀

-------------------------------------------------
 Role name:     ansible-role-tui
 Description:   Customization of linux TUI when accessing via SSH
 Creator:       cesargmm
 Version:       1.0.0
-------------------------------------------------
```

> [!WARNING]
> This role override your current .profile file. If you don't want to lose your current configuration, please
> make a backup of your current .profile file.

## Requirements

* Bash shell

## Role variables

| Variable           | Description                          | Default              |
|--------------------|--------------------------------------|----------------------|
| color_schema       | Color schema for the Welcome message | `yellow`             |
| image_color_schema | Color schema for the image           | `{{ color_schema }}` |
| image_path         | Path to images folder                | `files/images/`      |
| image              | Name of the image file               | `jirachi`            |

## Example playbook

You can use the following playbook to test the role in your own machine:

```yaml
---
# playbooks/main.yml
- hosts: localhost
  roles:
    - ansible-role-tui
```

```yaml
---
# inventory/hosts.yml
[localhost]
127.0.0.1
```

Here are some ansible-playbook command examples:

```bash
ansible-playbook -K -c local -i inventory/hosts playbooks/main.yml
```
![img](_Attachments/example-jirachi.png)

```bash
ansible-playbook -K -c local -e color_schema=magenta -e image=mew -D -i inventory/hosts playbooks/main.yml
```
![img](_Attachments/example-mew.png)

```bash
ansible-playbook -K -c local -e image_color_schema=white -e color_schema=cyan -e image=lugia -D -i inventory/hosts playbooks/main.yml
```
![img](_Attachments/example-lugia.png)

> [!WARNING]
> This role won't work if your default shell is not `bash`.