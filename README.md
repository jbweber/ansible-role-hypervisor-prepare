# Ansible Role: hypervisor_prepare

Prepares a system for use as a hypervisor host by configuring sysctl settings, disabling zram, and managing firewall settings.

## Requirements

- systemd-based Linux distribution (Fedora, RHEL, CentOS)
- Ansible 2.9 or higher

## Role Variables

Available variables are listed below, along with default values (see [defaults/main.yml](defaults/main.yml)):

```yaml
# Enable nested virtualization support (allows VMs inside VMs)
hypervisor_prepare_enable_nested_virtualization: false

# Enable IP forwarding for VM networking
hypervisor_prepare_enable_ip_forward: true

# Disable zram to ensure all memory is available for VMs
hypervisor_prepare_disable_zram: true

# Disable firewalld (hypervisor networking often requires custom firewall rules)
hypervisor_prepare_disable_firewall: true
```

### Nested Virtualization

When `hypervisor_prepare_enable_nested_virtualization` is set to `true`, the role will:
- Detect CPU vendor (Intel or AMD)
- Configure KVM kernel module parameters for nested virtualization
- Create `/etc/modprobe.d/kvm-nested.conf` with appropriate settings
- Verify nested virtualization status (if KVM module is already loaded)

**Note:** Changes to KVM module parameters require a system reboot or manual module reload to take effect.

## What This Role Does

1. **Sysctl Settings**: Enables IP forwarding for virtual machine networking (configurable)
2. **Zram**: Disables zram to ensure all memory is available for VMs (configurable)
3. **Firewall**: Disables firewalld if present (configurable)
4. **Nested Virtualization**: Optionally enables nested virtualization support for running VMs inside VMs (disabled by default)

## Example Playbook

Basic usage with default settings:

```yaml
- hosts: hypervisors
  become: true
  roles:
    - role: hypervisor_prepare
```

Enable nested virtualization:

```yaml
- hosts: hypervisors
  become: true
  roles:
    - role: hypervisor_prepare
      vars:
        hypervisor_prepare_enable_nested_virtualization: true
```

Selective feature configuration:

```yaml
- hosts: hypervisors
  become: true
  roles:
    - role: hypervisor_prepare
      vars:
        hypervisor_prepare_enable_nested_virtualization: true
        hypervisor_prepare_enable_ip_forward: true
        hypervisor_prepare_disable_zram: false
        hypervisor_prepare_disable_firewall: false
```

## Sysctl Settings Applied

- `net.ipv4.ip_forward = 1`: Enables IPv4 packet forwarding for VM networking

## License

MIT

## Author Information

Created for preparing systems for hypervisor host deployment.
