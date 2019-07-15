# satellite6-aws-demo
Set of AWS CloudFormation templates and [Ansible](https://www.redhat.com/en/technologies/management/ansible) playbooks for creating a [Red Hat Satellite 6](https://www.redhat.com/en/technologies/management/satellite) Demo Environment on AWS. Can be used for Satellite 6 demonstation purposes or as a foundation for more complex Satellite 6 architectures on AWS. **Not suitable** for production depoyments without applying additional security controls.

## List of Satellite 6.5 demos
 - 1x Satellite, 3 additional subnets, 3x capsules, 1 Organization and 4 Locations
 - 1x Satellite, 3 load balanced capsules - TODO
 - 1x Satellite with external DBs on Amazon RDS (postgresql) and Amazon DocumentDB (mongodb) - TODO
## Prerequisites and assumptions
 - AWS Account with [Red Hat Cloud Access](https://access.redhat.com/public-cloud) enabled. Otherwise you need to use a different set of RHEL AMI images and pay for usage
 - Active Red Hat subscription for Red Hat Satellite 6 itself and Red Hat Enterprise Linux VMs
 - [aws-cli](https://github.com/aws/aws-cli) installed and configured
 - Not using Elastic IPs so don't expect it to work flawlessly after rebooting
## Installation and configuration

```
# aws cloudformation create-stack --stack-name SATELLITE6DEMO --template-body https://raw.githubusercontent.com/amasolov/satellite6-aws-demo/master/cloudformation/satellite-standalone-capsules.yaml --parameters file://satellite6-aws-demo-parameters.json

{
    "StackId": "arn:aws:cloudformation:ap-southeast-2:291717560534:stack/SATELLITE6DEMO/92b694e0-a6c8-11e9-b61b-0ad4f0e57aba"
}
```

## How to get access

## How to remove it from AWS
