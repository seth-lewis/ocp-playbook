# ocp-playbook v0.1.0
Ansible Playbook for Installing OCP on vSphere, version 0.1.0. Expect included playbooks to change frequently prior to 1.0.0.

## Notes
1. This playbook will not "provision" any resources, it will "configure" resources that you have already provisioned
1. Loadbalancers:
    1. If you have ONE loadbalancer, use the `ocp-installer-lb-playbook.yaml` file when executing your playbook. It will configure your loadbalancer and your installer.
    1. If you have multiple LB, use `ocp-installer-playbook.yaml` when executing your playbook. You will need configure your load balancers manually. 
1. This playbook will be executed from your local machine, not the install VM.

### Assumptions & Requirements
1. You are attempting to do this install on **_VMware vSphere_** under CSPLAB
1. This playbook assumes you have ansible installed on the local machine you are running the playbook from
1. This playbook also assumes you can ssh in to your remote machine without a password (using ssh keys, see "Setting up SSH key authentication" [here](https://opensource.com/article/17/7/automate-sysadmin-ansible))
1. Replace all `<text_here>` blocks including the `<` & `>` characters where found in each file. Any surrounding quote marks are intentional.
    1. To make life easy, search each file for `<`, that character is only used where text needs to be replaced by the user.
    1. If you have multiple loadbalancers, skip `haproxy.cfg`.
1. The files `append-bootstrap.ign` && `install-config.yaml` must be in the same folder as the playbook being executed, and must have those same names.
    1. inventory.cfg can technically be anywhere, if you move it, you must pass the full path in when executing. Example: `-i /path/to/inventory.cfg`
    1. If using the script that configures the loadbalancer `haproxy.cfg` is required to be in the same folder as well.

## Example 
```
ansible-playbook -i inventory.cfg -u <remote_user_name> \
    -e ansible_become_pass=<remote_sudo_password> ocp-installer-playbook.yaml
```

### How To
1. If you set up your remote machine to sudo without a password for the user, the `-e ansible_become_pass=<remote_sudo_password>` line can be removed from the script.
    1. Note this is not logging on without a password, this means if you were logged on, if you configured it so 'sudo' can be executed without a password prompt.
    1. This step will probably be updated to use a vault in future versions.
1. In the last couple steps of this script, it will pause. You will have your base64 files saved wherever you specified in the playbook. Add them to the "vApp Options - Ignition Config Data" for each machine.
    1. `append-bootstrap.base64` goes with your bootstrap machine
    1. `master.base64` goes with your master/control-plane machines
    1. `worker.base64` goes with your worker & storage machines
1. After Playbook Completion:
    1. Login to the installer and export your KUBECONFIG (`export KUBECONFIG=/opt/<name>/auth/kubeconfig`). After that, continue with whatever you would normally do from this point on.
