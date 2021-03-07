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
  ansible-playbook -u root --ask-pass bootstrap_proxmox.yml
SSH password:

PLAY [all] *********************************************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************************
ok: [192.168.2.3]
ok: [192.168.2.2]

TASK [common : include_tasks] **************************************************************************************************************************************************************************************
included: /Users/jingkai/Documents/projects/ansible-proxmox/roles/common/tasks/01-update-apt-sources.yaml for 192.168.2.2, 192.168.2.3

TASK [common : Remove enterprise proxmox repository to get apt-get update to work without errors] ******************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

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
changed: [192.168.2.2]
fatal: [192.168.2.3]: FAILED! => {"changed": true, "cmd": "qm list | grep 1286", "delta": "0:00:00.896309", "end": "2021-03-07 19:06:38.545242", "msg": "non-zero return code", "rc": 1, "start": "2021-03-07 19:06:37.648933", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
...ignoring

TASK [common : Display all variables/facts known for a host] *******************************************************************************************************************************************************
skipping: [192.168.2.2]
skipping: [192.168.2.3]

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
fatal: [192.168.2.3]: FAILED! => {"changed": true, "cmd": "../build.sh proxmox", "delta": "0:00:04.318874", "end": "2021-03-07 19:06:45.907163", "msg": "non-zero return code", "rc": 1, "start": "2021-03-07 19:06:41.588289", "stderr": "--2021-03-07 19:06:43--  https://dl-cdn.alpinelinux.org/alpine/v3.12/releases/x86_64/alpine-virt-3.12.3-x86_64.iso\nResolving dl-cdn.alpinelinux.org (dl-cdn.alpinelinux.org)... 151.101.2.133, 151.101.66.133, 151.101.130.133, ...\nConnecting to dl-cdn.alpinelinux.org (dl-cdn.alpinelinux.org)|151.101.2.133|:443... connected.\nHTTP request sent, awaiting response... 304 Not Modified\nFile ‘/var/lib/vz/template/iso/alpine-virt-3.12.3-x86_64.iso’ not modified on server. Omitting download.", "stderr_lines": ["--2021-03-07 19:06:43--  https://dl-cdn.alpinelinux.org/alpine/v3.12/releases/x86_64/alpine-virt-3.12.3-x86_64.iso", "Resolving dl-cdn.alpinelinux.org (dl-cdn.alpinelinux.org)... 151.101.2.133, 151.101.66.133, 151.101.130.133, ...", "Connecting to dl-cdn.alpinelinux.org (dl-cdn.alpinelinux.org)|151.101.2.133|:443... connected.", "HTTP request sent, awaiting response... 304 Not Modified", "File ‘/var/lib/vz/template/iso/alpine-virt-3.12.3-x86_64.iso’ not modified on server. Omitting download."], "stdout": "\n==> Using VM ID: 1286 with default user: 'jingkai'\n\n\n=> Downloading ISO\n\n\n=> Downloading Ansible role\n\nStarting galaxy role install process\n- changing role ansible-alpine-initial from  to unspecified\n- extracting ansible-alpine-initial to /tmp/packer-template/proxmox-template-packer-master/alpine-3-amd64-proxmox/playbook/roles/ansible-alpine-initial\n- ansible-alpine-initial was installed successfully\n\n==> Build and create a Proxmox template.\n\nError: Failed to prepare build: \"proxmox\"\n\n1 error(s) occurred:\n\n* node must be specified\n\n\n\n==> Wait completed after 5 microseconds\n\n==> Builds finished but no artifacts were created.", "stdout_lines": ["", "==> Using VM ID: 1286 with default user: 'jingkai'", "", "", "=> Downloading ISO", "", "", "=> Downloading Ansible role", "", "Starting galaxy role install process", "- changing role ansible-alpine-initial from  to unspecified", "- extracting ansible-alpine-initial to /tmp/packer-template/proxmox-template-packer-master/alpine-3-amd64-proxmox/playbook/roles/ansible-alpine-initial", "- ansible-alpine-initial was installed successfully", "", "==> Build and create a Proxmox template.", "", "Error: Failed to prepare build: \"proxmox\"", "", "1 error(s) occurred:", "", "* node must be specified", "", "", "", "==> Wait completed after 5 microseconds", "", "==> Builds finished but no artifacts were created."]}

TASK [common : Clean artifacts] ************************************************************************************************************************************************************************************
skipping: [192.168.2.2]

PLAY RECAP *********************************************************************************************************************************************************************************************************
192.168.2.2                : ok=20   changed=4    unreachable=0    failed=0    skipped=8    rescued=0    ignored=0
192.168.2.3                : ok=23   changed=4    unreachable=0    failed=1    skipped=3    rescued=0    ignored=1

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
changed: [192.168.2.3]
changed: [192.168.2.2]

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
changed: [192.168.2.2]
fatal: [192.168.2.3]: FAILED! => {"changed": true, "cmd": "qm list | grep 1286", "delta": "0:00:00.881903", "end": "2021-03-07 19:07:53.764921", "msg": "non-zero return code", "rc": 1, "start": "2021-03-07 19:07:52.883018", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
...ignoring

TASK [common : Report the /boot/initramfs file status for latest installed kernel] *********************************************************************************************************************************
ok: [192.168.2.2] => {
    "msg": "host1"
}
ok: [192.168.2.3] => {
    "msg": "host2"
}

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
fatal: [192.168.2.3]: FAILED! => {"changed": true, "cmd": "../build.sh proxmox", "delta": "0:00:03.847562", "end": "2021-03-07 19:08:00.506338", "msg": "non-zero return code", "rc": 1, "start": "2021-03-07 19:07:56.658776", "stderr": "--2021-03-07 19:07:58--  https://dl-cdn.alpinelinux.org/alpine/v3.12/releases/x86_64/alpine-virt-3.12.3-x86_64.iso\nResolving dl-cdn.alpinelinux.org (dl-cdn.alpinelinux.org)... 151.101.2.133, 151.101.66.133, 151.101.130.133, ...\nConnecting to dl-cdn.alpinelinux.org (dl-cdn.alpinelinux.org)|151.101.2.133|:443... connected.\nHTTP request sent, awaiting response... 304 Not Modified\nFile ‘/var/lib/vz/template/iso/alpine-virt-3.12.3-x86_64.iso’ not modified on server. Omitting download.", "stderr_lines": ["--2021-03-07 19:07:58--  https://dl-cdn.alpinelinux.org/alpine/v3.12/releases/x86_64/alpine-virt-3.12.3-x86_64.iso", "Resolving dl-cdn.alpinelinux.org (dl-cdn.alpinelinux.org)... 151.101.2.133, 151.101.66.133, 151.101.130.133, ...", "Connecting to dl-cdn.alpinelinux.org (dl-cdn.alpinelinux.org)|151.101.2.133|:443... connected.", "HTTP request sent, awaiting response... 304 Not Modified", "File ‘/var/lib/vz/template/iso/alpine-virt-3.12.3-x86_64.iso’ not modified on server. Omitting download."], "stdout": "\n==> Using VM ID: 1286 with default user: 'jingkai'\n\n\n=> Downloading ISO\n\n\n=> Downloading Ansible role\n\nStarting galaxy role install process\n- changing role ansible-alpine-initial from  to unspecified\n- extracting ansible-alpine-initial to /tmp/packer-template/proxmox-template-packer-master/alpine-3-amd64-proxmox/playbook/roles/ansible-alpine-initial\n- ansible-alpine-initial was installed successfully\n\n==> Build and create a Proxmox template.\n\nError: Failed to prepare build: \"proxmox\"\n\n1 error(s) occurred:\n\n* node must be specified\n\n\n\n==> Wait completed after 6 microseconds\n\n==> Builds finished but no artifacts were created.", "stdout_lines": ["", "==> Using VM ID: 1286 with default user: 'jingkai'", "", "", "=> Downloading ISO", "", "", "=> Downloading Ansible role", "", "Starting galaxy role install process", "- changing role ansible-alpine-initial from  to unspecified", "- extracting ansible-alpine-initial to /tmp/packer-template/proxmox-template-packer-master/alpine-3-amd64-proxmox/playbook/roles/ansible-alpine-initial", "- ansible-alpine-initial was installed successfully", "", "==> Build and create a Proxmox template.", "", "Error: Failed to prepare build: \"proxmox\"", "", "1 error(s) occurred:", "", "* node must be specified", "", "", "", "==> Wait completed after 6 microseconds", "", "==> Builds finished but no artifacts were created."]}

TASK [common : Clean artifacts] ************************************************************************************************************************************************************************************
skipping: [192.168.2.2]

PLAY RECAP *********************************************************************************************************************************************************************************************************
192.168.2.2                : ok=21   changed=4    unreachable=0    failed=0    skipped=7    rescued=0    ignored=0
192.168.2.3                : ok=24   changed=4    unreachable=0    failed=1    skipped=2    rescued=0    ignored=1

❯  ansible-playbook -u root --ask-pass bootstrap_proxmox.yml
SSH password:

PLAY [all] *********************************************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************************
ok: [192.168.2.3]
ok: [192.168.2.2]

TASK [common : include_tasks] **************************************************************************************************************************************************************************************
included: /Users/jingkai/Documents/projects/ansible-proxmox/roles/common/tasks/01-update-apt-sources.yaml for 192.168.2.2, 192.168.2.3

TASK [common : Remove enterprise proxmox repository to get apt-get update to work without errors] ******************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

TASK [common : Add main respository into sources list] *************************************************************************************************************************************************************
ok: [192.168.2.3]
ok: [192.168.2.2]

TASK [common : Add updates respository into sources list] **********************************************************************************************************************************************************
ok: [192.168.2.2]
ok: [192.168.2.3]

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
changed: [192.168.2.3]
changed: [192.168.2.2]

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
changed: [192.168.2.2]
fatal: [192.168.2.3]: FAILED! => {"changed": true, "cmd": "qm list | grep 1286", "delta": "0:00:00.889222", "end": "2021-03-07 19:10:26.062343", "msg": "non-zero return code", "rc": 1, "start": "2021-03-07 19:10:25.173121", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
...ignoring

TASK [common : Test hosts list] ************************************************************************************************************************************************************************************
ok: [192.168.2.2] => {
    "msg": "192.168.2.2"
}
ok: [192.168.2.3] => {
    "msg": "192.168.2.3"
}

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
^C [ERROR]: User interrupted execution
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
