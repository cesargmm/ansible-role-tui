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

This role customizes a welcome message when accessing via SSH to your machine.

> [!WARNING]
> This role override your current .profile file as well as your /etc/update-motd.d/ directory. If you don't want to lose
> your current configuration, please
> make a backup of your current files.

## Requirements

* Bash shell

## Role variables

| Variable           | Description                          | Default        | Value                                                                                                    |
|--------------------|--------------------------------------|----------------|----------------------------------------------------------------------------------------------------------|
| color_schema       | Color schema for the Welcome message | `yellow`       | <code>blue&#124;brown&#124;cyan&#124;gray&#124;green&#124;magenta&#124;red&#124;white&#124;yellow</code> |
| image_color_schema | Color schema for the image           | `yellow`       | <code>blue&#124;brown&#124;cyan&#124;gray&#124;green&#124;magenta&#124;red&#124;white&#124;yellow</code> |
| image_path         | Path to images folder                | `files/images` | `string`                                                                                                 |
| image              | Name of a text-based image file      | `jirachi`      | `string`                                                                                                 |

> [!TIP]
> You can check the available images in the [images](https://github.com/cesargmm/ansible-role-tui/tree/main/files/images) folder.
> 
> Images have been generated with Pokémon Gen III 64x64 sprites from [Bulbapedia](https://archives.bulbagarden.net/wiki/Category:Ruby_and_Sapphire_sprites), using [Lachan's Braille-ASCII-Art Generator](https://lachlanarthur.github.io/Braille-ASCII-Art/).

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
  [ localhost ]
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