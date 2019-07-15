# satellite6-aws-demo
Set of AWS CloudFormation templates and [Ansible](https://www.redhat.com/en/technologies/management/ansible) playbooks for creating a [Red Hat Satellite 6](https://www.redhat.com/en/technologies/management/satellite) Demo Environment on AWS. Can be used for Satellite 6 demonstation purposes or as a foundation for more complex Satellite 6 architectures on AWS. 

**Not suitable for production deployments without applying additional security controls and configuration.***

## List of Satellite 6.5 demos
 - [Satellite with standalone Capsules](https://github.com/amasolov/satellite6-aws-demo/blob/master/cloudformation/satellite-standalone-capsules.yaml): 1x Satellite, 3 additional subnets, 3x capsules, 1 Organization and 4 Locations
 - 1x Satellite, 3 load balanced capsules - TODO
 - 1x Satellite with external DBs on Amazon RDS (postgresql) and Amazon DocumentDB (mongodb) - TODO
## Prerequisites and assumptions
 - AWS Account with [Red Hat Cloud Access](https://access.redhat.com/public-cloud) enabled. Otherwise you need to use a different set of RHEL AMI images and pay for usage
 - Active Red Hat subscription for Red Hat Satellite 6 itself and Red Hat Enterprise Linux VMs
 - [aws-cli](https://github.com/aws/aws-cli) installed and configured OR use AWS CloudFormation WebUI to create stacks
 - Not using Elastic IPs so don't expect it to work flawlessly after rebooting
## Installation and configuration

You can use AWS CloudFormation Web Console to create stacks by uploading the template. In case of using aws-cli, create a file with parameters as in [satellite6-aws-demo-parameters.json](https://github.com/amasolov/satellite6-aws-demo/blob/master/cloudformation/satellite6-aws-demo-parameters.json)

```
# cat satellite6-aws-demo-parameters.json
[
  {
    "ParameterKey": "RHSMActivationKey",
    "ParameterValue": "AK-alexeym-aws"
  }, 
  {
    "ParameterKey": "RHSMOrganization",
    "ParameterValue": "123456789"
  }, 
  {
    "ParameterKey": "DomainName",
    "ParameterValue": "alexeym.demo"
  },
  {
    "ParameterKey": "KeyName",
    "ParameterValue": "redhatkey"
  },
  {
    "ParameterKey": "RHSMUsername",
    "ParameterValue": "alexeym@alexeym.demo"
  },
  {
    "ParameterKey": "RHSMPassword",
    "ParameterValue": "MyPassword"
  },
  {
    "ParameterKey": "AWSSatelliteEC2Type",
    "ParameterValue": "r5.large"
  },
  {
    "ParameterKey": "AWSCapsuleEC2Type",
    "ParameterValue": "t3.large"
  },
  {
    "ParameterKey": "AWSAZ",
    "ParameterValue": "1"
  },
  {
    "ParameterKey": "RHSMPOOLID",
    "ParameterValue": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
  },
  {
    "ParameterKey": "RHSMPOOLQTY",
    "ParameterValue": "50"
  },
  {
    "ParameterKey": "RHSMCAPSULESUBSCRIPTIONNAME",
    "ParameterValue": "My Satellite Subscription Name"
  }
]
# aws cloudformation create-stack --stack-name SATELLITE6DEMO --template-body https://raw.githubusercontent.com/amasolov/satellite6-aws-demo/master/cloudformation/satellite-standalone-capsules.yaml --parameters file://satellite6-aws-demo-parameters.json

{
    "StackId": "arn:aws:cloudformation:ap-southeast-2:291717560534:stack/SATELLITE6DEMO/92b694e0-a6c8-11e9-b61b-0ad4f0e57aba"
}
```

## How to get access

```
### Wait till your demo stack is ready
# aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE

### Get information about your stack. It should contain a public Satellite hostname
# aws cloudformation describe-stacks --stack-name SATELLITE6DEMO

### Use Satellite public DNS name to access the instance over SSH using your EC2 key
ssh ec2-user@<EC2PublicHostname>

### Use admin/redhat as username/password to access Satellite WebUI on https://<EC2PublicHostname
```

## How to remove it from AWS
```
# aws cloudformation delete-stack --stack-name SATELLITE6DEMO
```