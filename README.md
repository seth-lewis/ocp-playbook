# ocp-playbook v0.1.0
Ansible Playbook for Installing OCP on vSphere, version 0.1.0. Expect scripts included to change


## Assumptions & Requirements
1. You are attempting to do this install on **_VMware vSphere_** under CSPLAB
1. This playbook assumes you have ansible installed on the local machine you are running the playbook from
1. This playbook also assumes you can ssh in to your remote machine without a password (using ssh keys, see here)
1. Replace all `<text_here>` blocks including the `<` & `>` characters where found in each file. Any surrounding quote marks are intentional.
    1. To make life easy, search each file for `<`, that character is only used where text is being replaced

## example 
```
ansible-playbook -i inventory.cfg -u <remote_user_name> -e ansible_become_pass=<remote_sudo_password> vOCP-Configure-playbook.yml
```
#### How To / Notes
1. At this time, this playbook will not set up your loadbalancer. There is a sample `haproxy.cfg` included, however it is for if you only have one load balancer. Many have multiple LBs, which is why I havent added that into the playbook just yet.
1. If you set up your remote machine to sudo without a password for the user, the `-e ansible_become_pass=<remote_sudo_password>` line can be removed from the script
1. In the last couple steps of this script, it will pause. You will have your base64 files saved wherever you specified in the playbook. Add them to the "vApp Options - Ignition Config Data" for each machine.
    1. `append-bootstrap.base64` goes with your bootstrap machine
    1. `master.base64` goes with your master/control-plane machines
    1. `worker.base64` goes with your worker & storage machines