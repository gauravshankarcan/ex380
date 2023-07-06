./setup.sh -e ignore_preflight_errors=true


skopeo copy docker-archive:///tmp/ee-29-rhel8.tar docker://hub.lab.example.com/ansible-automation-platform-22/ee-29-rhel8:1.0

```yaml
  vars:
    automation_controller_user: admin 1
    automation_controller_pass: redhat
    automation_controller_host: automation.example.com
    automation_controller_job: Demo%20Job%20Template 2

  tasks:
    - name: Launch a new Job
      uri: 3
        url: https://{{ automation_controller_host }}/api/v2/job_templates/{{ automation_controller_job }}/launch/
        method: POST
        validate_certs: no
        return_content: yes
        user: "{{ automation_controller_user }}"
        password: "{{ automation_controller_pass }}"
        force_basic_auth: yes
        status_code: 201



    - name: Use the token
      uri:
        url: "https://{{ automation_controller_host }}/api/v2/job_templates/{{ template_name | urlencode }}/launch/"
        method: POST
        validate_certs: false
        return_content: true
        status_code: 201
        headers:
          Authorization: "Bearer {{ response['json']['token'] }}" 3
          Content-Type: "application/json"
      register: launch
```


/usr/bin/automation-controller-service              
awx-manage changepassword admin
awx-manage createsuperuser

 supervisorctl status

/var/log/tower/

/var/log/supervisor/


./setup.sh -b
./setup.sh -r


insights-client
insights-client --register