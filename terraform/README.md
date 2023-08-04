# deploy-web-aws-nolb

Deploy a load balanced website on AWS

# Terraform

## Variables

Define the following variables in variables.tf where CIDR is an IP and netmask

```
$ cat variables.tf 
variable "selfa" {
  description = "Home IP"
  type        = string
  default     = "CIDR"
}
variable "selfb" {
  description = "temple IP"
  type        = string
  default     = "CIDR"
}
variable "keyname" {
  description = "keyname"
  type        = string
  default     = "yourkey"
}
variable "instance_name" {
  description = "instance name"
  type        = string
  default     = "web"
}
```

## Infrastructure

* AWS EC2 instances x2 running nginx. EC2 instances are deployed in separate AZs in the same region.

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
