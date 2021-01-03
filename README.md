After installing proxmox ve, add to `hosts` and run with
```
 ansible-playbook -u root --ask-pass bootstrap_proxmox.yml
```

- [x] Comments out proxmox enterprise repository to prevent apt from failing
- [x] Upgrades/updates packages, installs vim, pip, git, and unzip.
- [x] Modifies path to include pip's install path.
- [x] Removes totally redundant subscription message. Shell command from [here](https://dannyda.com/2020/05/17/how-to-remove-you-do-not-have-a-valid-subscription-for-this-server-from-proxmox-virtual-environment-6-1-2-proxmox-ve-6-1-2-pve-6-1-2/)
- [x] Installs ansible on the proxmox host for us to utilise packer and automate template setup
- [x] Installs [packer](https://www.packer.io/) on the proxmox host
- [x] Download packer alpine template from [chriswayg](https://github.com/chriswayg/packer-proxmox-templates) and replace with updates for
    - alpine 3.12
    - public ssh key
    - username.