# Cloudformation


CloudFormation is an infrastructure automation platform for AWS that deploys AWS resources in a repeatable, testable and auditable manner. Main components of CloudFormation:

- **Stack** - is a collection of AWS resources that you can manage as a single unit. You can create, update, or delete a collection of resources by creating, updating, or deleting stacks.
- **Templates** - all the resources in a stack are defined by the stack's template. They are formatted text files in JSON or YAML. 
- **Parameters** - enable you to input custom values to your template each time you create or update a stack.

![CloudFormation](https://www.techgeeknext.com/img/aws/cloudformation.png)


## Preparing the environment

1.	Create directories for templates and customizable parameters
2.	Install AWS CLI version 2 and configured AWS credentials


## Creating CloudFormation templates

### Important note!

For the name of the template file and parameters file, we use the names of the stack.:
```
template file - ${STACK}.yml
parameters file - ${STACK}-parameters.json
```
For example:

- for template file vpc.yml - vpc is stack name
- for parameters file vpc-parameters.json - vpc is stack name


## Creating Cloudformation parameters

The default parameters are specified inside the template file:
```
Parameters:
  VpcCIDR:
    Default: 10.0.0.0/16
    Description: Please enter the IP range for this VPC
    Type: String
  
  PublicSubnet1CIDR:
    Default: 10.0.0.0/22
    Description: Please enter the IP range for this public subnet 1
    Type: String

  PublicSubnet2CIDR:
    Default: 10.0.4.0/22
    Description: Please enter the IP range for this public subnet 2
    Type: String

  PrivateSubnet1CIDR:
    Default: 10.0.8.0/22
    Description: Please enter the IP range for this private subnet 1
    Type: String
```

And custom parameters are specified inside the parameters file:
```
[
    {
      "ParameterKey": "InstanceType",
      "ParameterValue": "t3.xlarge"
    }
]
```


## Deploying CloudFormation stack
Deploy vpc stack: 

```
STACK=vpc 
aws cloudformation deploy --stack-name ${STACK} --template-body templates/${STACK}.yml --parameters file://parameters/${STACK}-parameters.json

```
Check the deploy:

```
STACK=vpc 
aws cloudformation describe-stacks --stack-name ${stack} --output table --color on --query "Stacks[0].Parameters[]"
aws cloudformation describe-stacks --stack-name ${stack} --output table --color on --query "Stacks[0].Outputs[]"
aws cloudformation describe-stacks --stack-name ${stack} | grep StackDriftStatus
aws cloudformation describe-stack-resource-drifts --stack-name ${stack}
aws cloudformation cancel-update-stack --stack-name ${stack}

```


## Useful links:
1.	https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html 
2.	https://github.com/awslabs/aws-cloudformation-templates/blob/master/community/codestar/custom-ci-cd-pipeline/template.yml 
3.	https://dev.to/tiamatt/hands-on-aws-cloudformation-part-3-intrinsic-functions-in-action-5hj2 
4.	https://github.com/awslabs/ec2-spot-labs/blob/master/ec2-spot-instance-launch-templates/ec2-spot-instance-launch-templates.yaml 
5.	https://dev.to/aws-builders/create-aws-ecr-repository-using-cloudformation-55km 