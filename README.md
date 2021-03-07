Usage:
Prepare a group vars like the following
```
---
packer_version: 1.6.6
packer_download_directory: /tmp/packer
packer_template_version: master
alpine_template_dir: alpine-3-amd64-proxmox
proxmox_password: <pmx password>
vm_ssh_password: <password>
default_vm_id: 1286
```

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

Packer installation templates will take awhile to run. You can watch the magic from Proxmox's web interface.

The run log should look similar to this
```
❯  ansible-playbook -u root --ask-pass bootstrap_proxmox.yml
SSH password:

PLAY [all] *********************************************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************************
ok: [192.168.2.3]
ok: [192.168.2.2]

TASK [common : include_tasks] **************************************************************************************************************************************************************************************
included: /Users/jingkai/Documents/projects/ansible-proxmox/roles/common/tasks/01-update-apt-sources.yaml for 192.168.2.2, 192.168.2.3

TASK [common : Remove enterprise proxmox repository to get apt-get update to work without errors] ******************************************************************************************************************
ok: [192.168.2.3]
ok: [192.168.2.2]

TASK [common : Add main respository into sources list] *************************************************************************************************************************************************************
ok: [192.168.2.3]
ok: [192.168.2.2]

TASK [common : Add updates respository into sources list] **********************************************************************************************************************************************************
ok: [192.168.2.3]
ok: [192.168.2.2]

TASK [common : Add security respository into sources list] *********************************************************************************************************************************************************
ok: [192.168.2.3]
ok: [192.168.2.2]

TASK [common : include_tasks] **************************************************************************************************************************************************************************************
included: /Users/jingkai/Documents/projects/ansible-proxmox/roles/common/tasks/02-update-apt-packages.yaml for 192.168.2.2, 192.168.2.3

TASK [common : Update and upgrade apt packages] ********************************************************************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Install vim and pip] ********************************************************************************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : include_tasks] **************************************************************************************************************************************************************************************
included: /Users/jingkai/Documents/projects/ansible-proxmox/roles/common/tasks/03-remove-subscription-message.yaml for 192.168.2.2, 192.168.2.3

TASK [common : Remove subscription message from Proxmox VE] ********************************************************************************************************************************************************
[WARNING]: Consider using the replace, lineinfile or template module rather than running 'sed'.  If you need to use command because replace, lineinfile or template is insufficient you can add 'warn: false' to
this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.
changed: [192.168.2.2]
changed: [192.168.2.3]

TASK [common : include_tasks] **************************************************************************************************************************************************************************************
included: /Users/jingkai/Documents/projects/ansible-proxmox/roles/common/tasks/04-packer-bootstrap.yaml for 192.168.2.2, 192.168.2.3

TASK [common : Get python base path] *******************************************************************************************************************************************************************************
changed: [192.168.2.2]
changed: [192.168.2.3]

TASK [common : Check if pip3 path is already defined] **************************************************************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Insert pip3 path if not exist.] *********************************************************************************************************************************************************************
skipping: [192.168.2.2]
skipping: [192.168.2.3]

TASK [common : Install ansible] ************************************************************************************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Install j2cli] **************************************************************************************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Install py-bcrypt] **********************************************************************************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Check whether packer has been installed and is on system path] **************************************************************************************************************************************
changed: [192.168.2.2]
changed: [192.168.2.3]

TASK [common : Unarchive Packer] ***********************************************************************************************************************************************************************************
skipping: [192.168.2.2]
skipping: [192.168.2.3]

TASK [common : include_tasks] **************************************************************************************************************************************************************************************
included: /Users/jingkai/Documents/projects/ansible-proxmox/roles/common/tasks/05-create-alpine-template.yaml for 192.168.2.2, 192.168.2.3

TASK [common : Check whether template already exists.] *************************************************************************************************************************************************************
fatal: [192.168.2.3]: FAILED! => {"changed": true, "cmd": "qm list | grep 1286", "delta": "0:00:00.882935", "end": "2021-03-07 19:11:06.613479", "msg": "non-zero return code", "rc": 1, "start": "2021-03-07 19:11:05.730544", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
...ignoring
changed: [192.168.2.2]

TASK [common : Create a temporary location to store the packer template] *******************************************************************************************************************************************
skipping: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Download packer template from github] ***************************************************************************************************************************************************************
skipping: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Unarchive alpine packer template] *******************************************************************************************************************************************************************
skipping: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Execute ansible packer script.] *********************************************************************************************************************************************************************
skipping: [192.168.2.2]
changed: [192.168.2.3]

TASK [common : Clean artifacts] ************************************************************************************************************************************************************************************
skipping: [192.168.2.2]
changed: [192.168.2.3]

PLAY RECAP *********************************************************************************************************************************************************************************************************
192.168.2.2                : ok=20   changed=4    unreachable=0    failed=0    skipped=7    rescued=0    ignored=0
192.168.2.3                : ok=25   changed=6    unreachable=0    failed=0    skipped=2    rescued=0    ignored=1
```
