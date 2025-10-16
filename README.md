# Ansible Role: hypervisor_prepare

Prepares a system for use as a hypervisor host by configuring sysctl settings, disabling zram, and managing firewall settings.

## Requirements

- systemd-based Linux distribution (Fedora, RHEL, CentOS)
- Ansible 2.9 or higher

## Role Variables

This role does not require any variables to be set. All configuration is applied with sensible defaults.

## What This Role Does

1. **Sysctl Settings**: Enables IP forwarding for virtual machine networking
2. **Zram**: Disables zram to ensure all memory is available for VMs
3. **Firewall**: Disables firewalld if present (hypervisor networking often requires custom firewall rules)

## Example Playbook

```yaml
- hosts: hypervisors
  become: true
  roles:
    - role: hypervisor_prepare
```

## Sysctl Settings Applied

- `net.ipv4.ip_forward = 1`: Enables IPv4 packet forwarding for VM networking

## License

MIT

## Author Information

Created for preparing systems for hypervisor host deployment.
