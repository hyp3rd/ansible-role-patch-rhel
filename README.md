Ansible Role Patch RHEL
=======================

This Ansible Role enables to patch RHEL based (e.g., Red Hat Linux Fedora, and CentOS) systems.
The code allows to tune the patching strategy by enabling and disabling `yum` options as follows:

- `patch_rhel_update_cache: True`: update the **yum** cache before applying the updates.
- `patch_rhel_security: True`: apply only security fixes.
- `patch_rhel_bugfix: True`: apply only bugzillas fixes.

It is possible exclude packages `patch_rhel_exclude_list: kernel*, nginx` and to pin specific versions leveraging the **yum-plugin-versionlock** as shown below:

```bash
### Ensures yum-plugin-versionlock is present
patch_rhel_ensure_versionlock: True
### Call back to `yum versionlock` {{ package }}
# by passing a list of packages/versions to lock
patch_rhel_versionlock_packages:
   - <package_name>
```

Optionally, it is possible to generate reports: `patch_rhel_generate_reports: True` and send em notifying the following endpoints:

- Slack: `patch_rhel_notify_slack: True`;
- e-mail `patch_rhel_notify_email: True`;
- Custom webhook `patch_rhel_custom_hook: True`.

Reports notifications must be enabled `patch_rhel_notify_reports: True` combined with one or more of the above methods.

Role Variables
--------------

```yaml
---
# defaults file for patch-rhel

patch_rhel_ensure_versionlock: True

# yum command settings
patch_rhel_update_cache: True
patch_rhel_security: True
patch_rhel_bugfix: True

patch_rhel_exclude_list: kernel* # kernel*,foo*

patch_rhel_autoremove: True

patch_rhel_generate_reports: False
patch_rhel_notify_reports: False

# patch_rhel_versionlock_packages:
  #  - <package_name>

patch_rhel_notify_slack: False
patch_rhel_slack_token: ""

patch_rhel_notify_email: False
patch_rhel_email_notify_host: your_mail_host
patch_rhel_email_notify_server_port: "465"
patch_rhel_email_notify_server_username: ""
patch_rhel_email_notify_server_password: ""
patch_rhel_email_notify_from: reporting@example.com
patch_rhel_email_notify_to: devops@example.com
patch_rhel_email_notify_cc: risk@example.com
patch_rhel_email_notify_subject: "Infra Patching Report"
patch_rhel_email_notify_server_secure: starttls

patch_rhel_custom_hook: False
patch_rhel_custom_hook_url: ""
patch_rhel_custom_hook_token: ""
patch_rhel_custom_hook_method: POST
patch_rhel_custom_hook_body_format: json
patch_rhel_custom_hook_content_type: "application/json"
patch_rhel_custom_hook_expected_status_code: 200
```

Example Playbook
----------------

```yaml
- hosts:
    - all
  become: True
  gather_facts: True
  roles:
    - role: ansible-role-patch-rhel
      patch_rhel_generate_reports: True
      patch_rhel_notify_reports: True
      patch_rhel_notify_slack: True
      patch_rhel_slack_token: your_slack_token
```

License
-------

BSD

Author Information
------------------

[Francesco Cosentino](https://www.linkedin.com/in/francesco-cosentino/)

I'm a surfer, a crypto trader, and a DevSecOps Engineer with 15 years of experience designing highly-available distributed production environments and developing cloud-native apps in public and private clouds.
