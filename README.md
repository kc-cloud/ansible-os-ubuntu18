Ubuntu 18.04 CIS Benchmark
==========================
Configure Ubuntu 18.04 machine to be CIS compliant. Level 1 and 2 findings will be corrected by default. It's based on [CIS Ubuntu Benchmark v2.0.1 - 01-03-2020 ](https://www.cisecurity.org/cis-benchmarks/).


Role Variables
--------------
There are many role variables defined in defaults/main.yml. This list shows the most important.

**ubuntu1804cis_section1**: CIS - General Configurations (Section 1) (Default: true)

**ubuntu1804cis_section2**: CIS - Services Configurations (Section 2) (Default: true)

**ubuntu1804cis_section3**: CIS - Network Configurations (Section 3) (Default: true)

**ubuntu1804cis_section4**: CIS - Logging and Auditing Configurations (Section 4) (Default: true)

**ubuntu1804cis_section5**: CIS - Access, Authentication and Authorization Configurations (Section 5) (Default: true)

**ubuntu1804cis_section6**: CIS - System Maintenance Configurations (Section 6) (Default: true)  

##### Disable all selinux functions
`ubuntu1804cis_selinux_disable: false`

##### Service variables:
###### These control whether a server should or should not be allowed to continue to run these services

```
ubuntu1804cis_avahi_server: false  
ubuntu1804cis_cups_server: false  
ubuntu1804cis_dhcp_server: false  
ubuntu1804cis_ldap_server: false  
ubuntu1804cis_telnet_server: false  
ubuntu1804cis_nfs_server: false  
ubuntu1804cis_rpc_server: false  
ubuntu1804cis_ntalk_server: false  
ubuntu1804cis_rsyncd_server: false  
ubuntu1804cis_tftp_server: false  
ubuntu1804cis_rsh_server: false  
ubuntu1804cis_nis_server: false  
ubuntu1804cis_snmp_server: false  
ubuntu1804cis_squid_server: false  
ubuntu1804cis_smb_server: false  
ubuntu1804cis_dovecot_server: false  
ubuntu1804cis_httpd_server: false  
ubuntu1804cis_vsftpd_server: false  
ubuntu1804cis_named_server: false  
ubuntu1804cis_bind: false  
ubuntu1804cis_vsftpd: false  
ubuntu1804cis_httpd: false  
ubuntu1804cis_dovecot: false  
ubuntu1804cis_samba: false  
ubuntu1804cis_squid: false  
ubuntu1804cis_net_snmp: false  
```  

##### Designate server as a Mail server
`ubuntu1804cis_is_mail_server: false`


##### System network parameters (host only OR host and router)
`ubuntu1804cis_is_router: false`  


##### IPv6 required
`ubuntu1804cis_ipv6_required: true`  


##### AIDE
`ubuntu1804cis_config_aide: true`

###### AIDE cron settings
```
ubuntu1804cis_aide_cron:
  cron_user: root
  cron_file: /etc/crontab
  aide_job: '/usr/sbin/aide --check'
  aide_minute: 0
  aide_hour: 5
  aide_day: '*'
  aide_month: '*'
  aide_weekday: '*'  
```


##### Set to 'true' if X Windows is needed in your environment
`ubuntu1804cis_xwindows_required: no`


##### Client application requirements
```
ubuntu1804cis_openldap_clients_required: false
ubuntu1804cis_telnet_required: false
ubuntu1804cis_talk_required: false  
ubuntu1804cis_rsh_required: false
ubuntu1804cis_ypbind_required: false
```

##### Time Synchronization
```
ubuntu1804cis_time_synchronization: chrony
ubuntu1804cis_time_Synchronization: ntp

ubuntu1804cis_time_synchronization_servers:
  - uri: "0.pool.ntp.org"
    config: "minpoll 8"
  - uri: "1.pool.ntp.org"
    config: "minpoll 8"
  - uri: "2.pool.ntp.org"
    config: "minpoll 8"
  - uri: "3.pool.ntp.org"
    config: "minpoll 8"

```
##### - name: "SCORED | 1.1.5 | PATCH | Ensure noexec option set on /tmp partition"
It is not implemented, noexec for /tmp will disrupt apt. /tmp contains executable scripts during package installation
```

```  
##### 1.5.3 | PATCH | Ensure authentication required for single user mode
It is disabled by default as it is setting random password for root. To enable it set:
```yaml
ubuntu1804cis_rule_1_5_3: true
```
To use other than random password:
```yaml
ubuntu1804cis_root_password: 'new password'
```

##### 3.4.2 | PATCH | Ensure /etc/hosts.allow is configured
```
ubuntu1804cis_host_allow:
  - "10.0.0.0/255.0.0.0"  
  - "172.16.0.0/255.240.0.0"  
  - "192.168.0.0/255.255.0.0"    
```  

```
ubuntu1804cis_firewall: firewalld
ubuntu1804cis_firewall: iptables
```

##### 5.3.1 | PATCH | Ensure password creation requirements are configured
```
ubuntu1804cis_pwquality:
  - key: 'minlen'
    value: '14'
  - key: 'dcredit'
    value: '-1'
  - key: 'ucredit'
    value: '-1'
  - key: 'ocredit'
    value: '-1'
  - key: 'lcredit'
    value: '-1'
```



Example Playbook (playbook.yml)
-----------------

```
- name: Harden Server
  hosts: servers
  roles:
    - Ubuntu1804-CIS
```

Assuming you named the file `playbook.yml`, run it with:
```bash
ansible-playbook playbook.yml
```

Tags
----
Many tags are available for precise control of what is and is not changed.

Some examples of using tags:

```
    # Audit and patch the site
    ansible-playbook playbook.yml --tags="level1"
```
