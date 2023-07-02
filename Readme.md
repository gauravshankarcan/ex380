ansible-playbook playbook.yml -i inventory
ansible-navigator run playbook.yml -i inventory -m stdout --eei ee-supported-rhel8:latest --pp missing
ansible-navigator collections
ansible-navigator doc redhat.insights.insights_register --mode stdout
ansible-navigator config  -m stdout --eei ee-supported-rhel8:latest
ansible-navigator settings --effective --pp missing --eei ee-supported-rhel8



ansible-galaxy collection install community.crypto -p ~/myproject/collections/
ansible-galaxy collection install /tmp/community-dns-1.2.0.tar.gz
ansible-galaxy collection install http://www.example.com/redhat-insights-1.0.5.tar.gz
ansible-galaxy collection install git@github.com:ansible-collections/community.mysql.git
ansible-galaxy collection install -r collections/requirements.yml
ansible-galaxy collection list

ansible-vault view secret.yml --vault-password-file=secret-pass
ansible-doc -l | grep firewalld

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