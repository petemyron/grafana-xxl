# Ansible for dockerized grafana-xxl

This for your Ansible inspiration. Final implementation is up to you:

```
# Jan Garaj - www.monitoringartist.com
# Used Core Docker module - http://docs.ansible.com/ansible/docker_module.html
- name: grafana-xxl container
  docker:
    name: grafana-xxl
    image: monitoringartist/grafana-xxl:latest
    state: started
    pull: always
    restart_policy: unless-stopped
    ports:
    - "3000:3000"     
```

# Author

[Devops Monitoring Expert](http://www.jangaraj.com 'DevOps / Docker / Kubernetes / AWS ECS / Google GCP / Zabbix / Zenoss / Terraform / Monitoring'),
who loves monitoring systems, which start with letter Z. Those are Zabbix and Zenoss.

Professional devops / monitoring services:

[![Monitoring Artist](http://monitoringartist.com/img/github-monitoring-artist-logo.jpg)](http://www.monitoringartist.com 'DevOps / Docker / Kubernetes / AWS ECS / Google GCP / Zabbix / Zenoss / Terraform / Monitoring')
