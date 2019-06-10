# Ansible AWS Route53 Zone
An Ansible role that will create a record in Route53

## Requirements
AWS credentials and the correct permissions to create the resources

## Role Variables

The variables uses in this role are

| Variable Name | Required | Description | 
|----|----|----|
| `route53_zone_name`| **Yes** | The region to use |
| `route53_comment`| Optional | A comment attached to the DNS Zone |
| `route53_vpc_id`| Optional | The VPC id to use for the private zone. Only used when creating a private zone |
| `route53_vpc_region`| Optional | The region to use for the private zone. Only used when creating a private zone |

## Dependencies

None

## Example Playbook

### Download dependencies

#### Create requirements file

Create a `requirements.yml` file with the following contents
```
- src: https://github.com/maishsk/aws-route53-zone
  version: master
```

#### Download dependencies
Run the following command:
```
ansible-galaxy install -r requirements.yml --force -p .
```

### Create playbook
Create a `main.yaml` file with the following contents:
```
---
- name: Example playbook
  hosts: localhost
  connection: local
  gather_facts: false
  vars_files:
    - vars/vars.yml

  tasks:
  - name: Create Process
    include_role:
      name: "{{ item }}"
    with_items:
      - aws-route53-zone
    tags: [ 'never', 'create' ]

  - name: Rollback Process
    include_role:
      name: "{{ item }}"
    with_items:
      - aws-route53-zone
    tags: [ 'never', 'rollback' ]
```

Create a `vars/vars.yml` with the content similar to:
For a public zone:
```
route53_zone_name: maishsk.local
route53_comment: This is my zone
```
For a private zone:
route53_zone_name: maishsk.local
route53_comment: This is my zone
route53_vpc_id: vpc-1234567890
route53_vpc_region: us-east-2

## Running the playbook

To create the route53 zone

`ansible-playbook main.yml --tags create`

To remove the route53 zone

`ansible-playbook main.yml --tags rollback`

## License

BSD

## Author Information
This role was created by [Maish Saidel-Keesing](https://www.maishsk.com/), author of [The Cloud Walkabout](http://cloudwalkabout.com/).