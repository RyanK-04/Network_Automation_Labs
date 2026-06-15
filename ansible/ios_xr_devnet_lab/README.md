# CNS4107 Ansible Network Automation Lab

## Control Node
- OS: Ubuntu 24.04 LTS (Kubuntu)
- Python: 3.12.3
- Ansible Core: 2.21.0

## Target Device
- Platform: Cisco IOS XR (Catalyst 8000 / IOS XR Always-On)
- Hostname: NetGuard
- Version: 25.3.1 LNT
- Sandbox: Cisco DevNet Always-On (sandbox-iosxr-1.cisco.com)

## What Worked
- SSH connectivity to IOS XR Always-On sandbox
- Gathering device facts with cisco.iosxr.iosxr_facts
- Running show commands with cisco.iosxr.iosxr_command
- Backing up running configuration
- Rendering Jinja2 templates locally (loopbacks, management services)
- Gathering structured interface data with iosxr_interfaces
- Generating Markdown reports from inventory variables

## What Failed / Challenges
- Catalyst 8000 Always-On sandbox rejected valid credentials (broken sandbox state)
- Had to switch from IOS XE to IOS XR sandbox
- ansible.cfg ssh_args needed explicit PubkeyAuthentication=no to prevent key auth attempts
- show running-config | include syntax differs on IOS XR (requires 'formal' keyword)
- Playbook files in lab guide had spaces in filenames and YAML that needed correction
- Output files landed in wrong directory when playbooks run from inside playbooks/ folder
  (fixed using {{ playbook_dir }}/../ for all paths)

## What I Learned
- IOS XR and IOS XE have different CLI syntax (ipv4 address vs ip address, etc.)
- Ansible network_cli connection plugin uses Paramiko which requires explicit config
  to disable public key authentication
- group_vars/all.yml variables are merged with host_vars/devnet-router.yml at runtime
- Resource modules support rendered/gathered/parsed states that work without a device
- Always test raw SSH before debugging Ansible connection issues
- DevNet Always-On sandboxes are shared and can be broken by other users
