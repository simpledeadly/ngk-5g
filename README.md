# Ansible Playbook for setup 5G core

**Output:**

```bash
simpledeadly@ubuntu-22-v2:~$ ansible-playbook setup5.yaml
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Deploy Open5GS Core and OAI gNB] ********************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************
ok: [localhost]

TASK [1. Enable IPv4 forwarding] **************************************************************************************************
ok: [localhost]

TASK [2. Install apt prerequisites] ***********************************************************************************************
ok: [localhost]

TASK [3. Add MongoDB GPG key] *****************************************************************************************************
ok: [localhost]

TASK [4. Add MongoDB apt repo] ****************************************************************************************************
ok: [localhost]

TASK [5. Install MongoDB] *********************************************************************************************************
ok: [localhost]

TASK [6. Start MongoDB] ***********************************************************************************************************
ok: [localhost]

TASK [7. Add Open5GS PPA] *********************************************************************************************************
ok: [localhost]

TASK [8. Install Open5GS] *********************************************************************************************************
ok: [localhost]

TASK [9. Add NodeSource GPG key] **************************************************************************************************
ok: [localhost]

TASK [10. Add NodeSource repo] ****************************************************************************************************
ok: [localhost]

TASK [11. Install NodeJS] *********************************************************************************************************
ok: [localhost]

TASK [12. Install Open5GS WebUI] **************************************************************************************************
ok: [localhost]

TASK [13. Restart Open5GS] ********************************************************************************************************
changed: [localhost] => (item=open5gs-mmed)
changed: [localhost] => (item=open5gs-sgwud)
changed: [localhost] => (item=open5gs-nrfd)
changed: [localhost] => (item=open5gs-amfd)
changed: [localhost] => (item=open5gs-upfd)

TASK [14. Install OAI deps] *******************************************************************************************************
ok: [localhost]

TASK [15. Clone OAI repo] *********************************************************************************************************
changed: [localhost]

TASK [16. Add Ettus UHD PPA] ******************************************************************************************************
ok: [localhost]

TASK [17. Install UHD host] *******************************************************************************************************
ok: [localhost]

TASK [18. Build OAI deps] *********************************************************************************************************
changed: [localhost]

TASK [19. Compile OAI] ************************************************************************************************************
ok: [localhost]

TASK [20. Create UHD dir] *********************************************************************************************************
ok: [localhost]

TASK [21. Download UHD images] ****************************************************************************************************
[WARNING]: Consider using the get_url or uri module rather than running 'wget'.  If you need to use command because get_url or uri
is insufficient you can add 'warn: false' to this command task or set 'command_warnings=False' in ansible.cfg to get rid of this
message.
changed: [localhost]

PLAY RECAP ************************************************************************************************************************
localhost                  : ok=22   changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```