![ci](https://github.com/rafael-c-alexandre/ansible-role-security/actions/workflows/ci.yml/badge.svg)
![release](https://github.com/rafael-c-alexandre/ansible-role-security/actions/workflows/release.yml/badge.svg)

rafael-c-alexandre.security
===========================

An Ansible role that sets up basic security services on Debian servers. It modifies SSH configurations and sets up services such as Fail2Ban, automatic updates, and firewall rules through UFW.

Requirements
------------

- **Ansible Version**: Ensure you are running Ansible version 2.18 or higher.
- **Supported Systems**: This role is designed for Debian-based systems.

Role Variables
--------------

The following variables can be customized to tailor the role to your needs. Default values are defined in `defaults/main.yml`.

### SSH Configuration

- `security_ssh_enabled`: *(Default: `true`)* Whether to configure SSH settings.
- `security_ssh_password_authentication`: *(Default: `"no"`)* Enable or disable password authentication.
- `security_ssh_permit_root_login`: *(Default: `"no"`)* Whether to permit root login via SSH.
- `security_ssh_usedns`: *(Default: `"no"`)* Whether to use DNS to verify client hostnames.
- `security_ssh_permit_empty_password`: *(Default: `"no"`)* Whether to allow logins with empty passwords.
- `security_ssh_challenge_response_auth`: *(Default: `"no"`)* Enable or disable challenge-response authentication.
- `security_ssh_gss_api_authentication`: *(Default: `"no"`)* Enable or disable GSSAPI authentication.
- `security_ssh_x11_forwarding`: *(Default: `"no"`)* Enable or disable X11 forwarding.

### Fail2Ban Configuration

- `security_fail2ban_enabled`: *(Default: `true`)* Whether to install and configure Fail2Ban.
- `security_fail2ban_custom_configuration_template`: *(Default: `"jail.local.j2"`)* The template file used for custom Fail2Ban configurations.

### Automatic Updates

- `security_autoupdate_enabled`: *(Default: `true`)* Whether to enable automatic security updates.
- `security_autoupdate_blacklist`: *(Default: `[]`)* List of packages to exclude from automatic updates.
- `security_autoupdate_additional_origins`: *(Default: `[]`)* Additional origins to fetch updates from.
- `security_autoupdate_reboot`: *(Default: `"false"`)* Whether to automatically reboot the system after updates.
- `security_autoupdate_reboot_time`: *(Default: `"03:00"`)* The time of day when the system should reboot if required.
- `security_autoupdate_mail_to`: *(Default: `""`)* Email address to send update notifications. Leave empty to disable notifications.
- `security_autoupdate_mail_on_error`: *(Default: `true`)* Whether to send an email when an error occurs during updates.

### UFW (Uncomplicated Firewall) Configuration

- `security_ufw_enabled`: *(Default: `true`)* Whether to install and configure UFW.
- `security_ufw_logging`: *(Default: `"low"`)* The logging level for UFW.
- `security_ufw_default_incoming_policy`: *(Default: `"deny"`)* Default policy for incoming traffic.
- `security_ufw_default_outgoing_policy`: *(Default: `"allow"`)* Default policy for outgoing traffic.
- `security_ufw_rules`: *(Default: see below)* A list of firewall rules to apply.

#### Default UFW Rules:
```yaml
security_ufw_rules:
  - rule: allow
    to_port: "{{ security_ansible_port }}"
    protocol: tcp
    comment: allow-ssh
```

Dependencies
------------

This role has no external dependencies.


Example Playbook
----------------

    - hosts: servers
      become: true
      roles:
        - role: rafael-c-alexandre.security
          vars:
            security_ssh_permit_root_login: "no"
            security_ssh_password_authentication: "no"
            security_fail2ban_enabled: true
            security_fail2ban_custom_configuration_template: "custom-jail.local.j2"
            security_autoupdate_enabled: true
            security_autoupdate_reboot: "true"
            security_ufw_enabled: true
            security_ufw_rules:
              - rule: allow
                to_port: 2222
                protocol: tcp
                comment: allow-custom-ssh
              - rule: allow
                to_port: 80
                protocol: tcp
                comment: allow-http
              - rule: allow
                to_port: 443
                protocol: tcp
                comment: allow-https

License
-------

MIT
