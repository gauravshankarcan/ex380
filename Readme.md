ANSIBLE_NAVIGATOR_CONFIG=~/.ansible-navigator.yml

ansible-playbook playbook.yml -i inventory
ansible-navigator run playbook.yml -i inventory -m stdout --eei ee-supported-rhel8:latest --pp missing
ansible-navigator collections
ansible-navigator doc redhat.insights.insights_register --mode stdout
ansible-navigator config  -m stdout --eei ee-supported-rhel8:latest
ansible-navigator settings --effective
 --pp missing --eei ee-supported-rhel8
ansible-navigator doc --mode stdout --type inventory --list
ansible-navigator doc --mode stdout --type inventory --list
ansible-navigator inventory --mode stdout -i inventory --list
ansible-navigator inventory --mode stdout --graph webservers   
ansible-navigator run -m stdout playbook.yml -i inventory --list-tags

For dynamic inventroy the groups need to exits in the hosts/ini file even if the hosts dont exist

ansible-galaxy collection install community.crypto -p ~/myproject/collections/
ansible-galaxy collection install /tmp/community-dns-1.2.0.tar.gz
ansible-galaxy collection install http://www.example.com/redhat-insights-1.0.5.tar.gz
ansible-galaxy collection install git@github.com:ansible-collections/community.mysql.git
ansible-galaxy collection install -r collections/requirements.yml
ansible-galaxy collection list

ansible-vault view secret.yml --vault-password-file=secret-pass
ansible-doc -l | grep firewalld
ansible-inventory -i inventory.ini -y --list > inventory.yaml

python -m pydoc jinja2.filters

subscription-manager register
subscription-manager repos --enable ansible-automation-platform-2.2-for-rhel-8-x86_64-rpms
yum install ansible-navigator

git config --global push.default simple
git checkout -b feature/3
git push --set-upstream origin feature/3
git add .gitignore

anisble facts --> ansible_facts
9090 port cockpit


naming of variables  --> should follow grouping like web_ ,dbservers_

├── dbservers.yml
├── inventories/
│   ├── prod/
│   │   ├── group_vars/
│   |       └── all
│   │   ├── host_vars/
│   │   └── inventory/
│   └── stage/
│       ├── group_vars/
│       ├── host_vars/
│       └── inventory/
├── roles/
│   └── std_server/
├── site.yml
├── storage.yml
└── webservers.yml


ansible_host
ansible_port
ansible_user
ansible_become_user
ansible_python_interpreter

Identifying the Current Host by Using Variables
inventory_hostname
ansible_host
ansible_facts['hostname']
ansible_facts['fqdn']



```yaml
    - name: Ensure the firewall ports are opened
      ansible.builtin.include_role:
        name: firewall
      vars:
        firewall_rules:
```        

Configuration directive	Command-line option
become	--become or -b
become_method	--become-method=BECOME_METHOD
become_user	--become-user=BECOME_USER
become_password	--ask-become-pass or -K


pre_tasks
Handlers that are notified in the pre_tasks section
roles
tasks
Handlers that are notified in the roles and tasks sections
post_tasks


```yaml

    - name: Deploying the configuration file
      ansible.builtin.template:
        src: api-server.cfg.j2
        dest: /etc/api-server.cfg
      notify: Restart api server

    - name: Running all notified handlers
      ansible.builtin.meta: flush_handlers


  handlers:
    # Listening to the "My handlers" event
    - name: Listening to a notification
      ansible.builtin.debug:
        msg: First handler was notified
      listen: My handlers             -------------------------
 ```     


order: sorted
        inventory
        reverse_inventory
        sorted
        reverse_sorted
        shuffle

``` yaml

    - name: httpd is installed
      ansible.builtin.yum:
        name: httpd
        state: latest
      tags: webserver
```      


The tagged tag runs any resource with an explicit tag.
The untagged tag runs any resource that does not have an explicit tag, and excludes all tagged resources.
The all tag includes all tasks in the play, whether they have a tag or not. This is the default behavior of Ansible.


```yaml
- name: A play that gathers some facts
  hosts: all
  gather_facts: false

  tasks:
    - name: Collect only network-related facts
      ansible.builtin.setup:
        gather_subset:
          - '!all'
          - '!min'
          - network

 all, min, hardware, network, virtual, ohai, and facter

```          


```yaml

  tasks:
    - name: Ensure the packages are installed
      ansible.builtin.yum:
        name:
          - httpd
          - mod_ssl
          - httpd-tools
          - mariadb-server
          - mariadb
          - php
          - php-mysqlnd
        state: present

  
  tasks:
    - name: Ensure web content is updated
      ansible.posix.synchronize:
        src: web_content/
        dest: /var/www/html
        
        

    - name: Ensure proper configuration of the Apache HTTP Server
      ansible.builtin.lineinfile:
        dest: /etc/httpd/conf/httpd.conf
        regexp: "{{ item.regexp }}"
        line: "{{ item.line }}"
        state: present
      loop:
       - regexp: '^Listen 80$'
         line: 'Listen 8181'
       - regexp: '^ServerAdmin root@localhost'
         line: 'ServerAdmin support@example.com'
       - regexp: '^DocumentRoot "/var/www/html"'
         line: 'DocumentRoot "/var/www/web"'
       - regexp: '^<Directory "/var/www/html">'
         line: '<Directory "/var/www/web">'


- name: Perform smoke test
  ansible.builtin.uri:
    url: "https://{{ blue }}/status"
    return_content: true
  register: smoke_test
  until: "'STATUS_OK' in smoke_test['content']"
  retries: 12
  delay: 10


- name: Second task
  hosts: servera.lab.example.com
  gather_facts: false
  become: true

  tasks:
    - name: Add users and put them in the right groups
      ansible.builtin.user:
        name: "{{ item[0]['name'] }}"
        append: true
        groups: "{{ item[1] }}"
        state: present
      loop: "{{ my_users | subelements('my_groups') }}"
```


```
| default(omit)
{{ pattern | regex_search('test') | default('MESSAGE', true) }}"  Although the regex_search filter returns an empty string, the default filter is used because it includes true.
{{ [2, 4, 6, 8, 10, 12] | sum }}
{{ 1764 | root }}
"{{ [ 2, 4, 6, 8, 10, 12 ] | length }} is eq( 6 )"
"{{ [ 2, 4, 6, 8, 10, 12 ] | first }} is eq( 2 )"
"{{ [ 2, 4, 6, 8, 10, 12 ] | last }} is eq( 12 )"
{{ ['Douglas', 'Marvin', 'Arthur'] | random }}
"{{ [ 2, 4, 6, 8, 10 ] | reverse }
"{{ [ 4, 8, 10, 6, 2 ] | sort }} 
"{{ [ 2, [4, [6, 8]], 10 ] | flatten }}
"{{ [ 1, 1, 2, 2, 2, 3, 4, 4 ] | unique }}
"{{ [2, 4, 6, 8, 10] | difference([2, 4, 6, 16]) }}
{{ {'A':1,'B':2} | combine({'B':4,'C':5}) }} 
"{{ characters_dict | dict2items }} 
"{{ characters_items | items2dict }} 
"'{{ 'Arthur' | hash('sha1') }}
'{{ 'âÉïôú' | b64encode }}
"'{{ 'w6LDicOvw7TDug==' | b64decode }}
{{ 'Marvin' | lower }}
"'{{ 'Marvin' | upper }}' is eq( 'MARVIN' )"
"'{{ 'marvin' | capitalize }}' is eq( 'Marvin' )"
"{{ query_results['json']['results'] | selectattr('name', '==', 'Control Plane Execution Environment') | map(attribute='id') | first }}"
"'{{ hosts | to_json }}' is eq( hosts_json )"
{{ apache_base_packages | union(apache_optional_packages) }}
{{ webapp_find_files['files'] | map(attribute='path') | list }}"
"{{ webapp_deployed_files | map('relpath', webapp_content_root_dir) | list }}"
"'{{ hosts | to_json }}


{{ lookup('ansible.builtin.file', 'my.file', errors='warn') }}

"{{ lookup('ansible.builtin.fileglob', '~/.bash*') }}" -->  "{{ query('fileglob', '~/.bash*') }}"



ansible_facts['dns']['nameservers']	The DNS name servers used for name resolution by the managed host (included in the min subset).
ansible_facts['domain']	The domain for the managed host.
ansible_facts['all_ipv4_addresses']	All the IPv4 addresses configured on the managed host.
ansible_facts['all_ipv6_addresses']	All the IPv6 addresses configured on the managed host.
ansible_facts['fqdn']	The fully qualified domain name (DNS name) of the managed host.
ansible_facts['hostname']	The unqualified hostname; the string in the FQDN before the first period.

{{ my_hosts_list | ansible.utils.ipaddr }}
{{ listips | ansible.utils.ipaddr('netmask') }}


| ansible.utils.ipaddr('host')
 | ansible.utils.ipaddr('net')
 | ansible.utils.ipaddr('private')
 | ansible.utils.ipaddr('public')
 ansible.utils.ipaddr('revdns')
  ansible.utils.ipaddr('network/prefix')
  ansible.utils.ipaddr('broadcast')

 {{ listips | ansible.utils.ipaddr | ansible.utils.ipwrap }}


       delegate_to: demo.lab.example.com
      delegate_facts: true



  serial: 2
  max_fail_percentage: 30%
  serial: 25%
  serial:
    - 1
    - 10%
    - 100%

- name: Reactivate Hosts
  ansible.builtin.shell: /sbin/activate.sh {{ active_hosts_string }}
  run_once: true
  delegate_to: monitor.example.com
  vars:
    active_hosts_string: "{{ ansible_play_batch | join(' ')}}"
