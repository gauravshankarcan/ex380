./setup.sh -e ignore_preflight_errors=true


skopeo copy docker-archive:///tmp/ee-29-rhel8.tar docker://hub.lab.example.com/ansible-automation-platform-22/ee-29-rhel8:1.0