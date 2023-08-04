# deploy-web-aws-nolb

Deploy a single instane website to AWS

# Terraform

## Infrastructure

* AWS EC2 instance running nginx on ports 80 and 443

## Interface

* Describe current infrastructure

```
cd terraform
tf show
```

# Ansible

## Inventory

* `ansible/inventory/aws_ec2.yml.sample` is a sample dynamic AWS inventory module for Ansible. Installation instructions: https://devopscube.com/setup-ansible-aws-dynamic-inventory/

## Playbooks

* letsencrypt: Check SSL certificate validity

```
ansible-playbook letsencrypt.yml -i inventory --tags test --ask-become-pass
```

* letsencrypt: Renew SSL certificate

```
ansible-playbook letsencrypt.yml -i inventory --tags enroll --ask-become-pass
```

* deploy-web: Deploy website

```
ansible-playbook deploy-web.yml -i inventory --tags all
```

* deploy-web: Restart nginx

```
ansible-playbook deploy-web.yml -i inventory --tags restartweb
```
