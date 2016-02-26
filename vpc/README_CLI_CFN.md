This document will talk about how to integrate aws cli with cfn templates with Example

AWS documentation link : http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html

1.) Details about creating VPC using aws cli

Template to Create VPC:
Name: VPCIGW.template

Input Parameters:
validCIDR:
VPCName:
IGWName:
DNSHostname:
DNSSupport:
Tenancy:

Description: This template will create a VPC along with InternetGateway

AWC cli command:
aws cloudformation create-stack --stack-name sandy-vpc-stack --template-body file:///data/codebase/opstree/OpstreeCfn/vpc/VPCIGW.template --parameters ParameterKey=validCIDR,ParameterValue=10.240.0.0/16 ParameterKey=VPCName,ParameterValue=VPC-SANDY ParameterKey=IGWName,ParameterValue=IGW-SANDY ParameterKey=DNSHostname,ParameterValue=true ParameterKey=DNSSupport,ParameterValue=true ParameterKey=Tenancy,ParameterValue=default

AWS cli command to get the Outputs
aws cloudformation describe-stacks --stack-name sandy-vpc-stack

2.) Details about creation of Public Route table

Template to create Public Route table:
VPCPubRouteTable.template

Input Parameters:
VpcId:
RouteTableName:
IDofIGW:

Description: This template will create a Public Route Table

AWC cli command:
aws cloudformation create-stack --stack-name sandy-vpc-pub-rt --template-body file:///data/codebase/opstree/OpstreeCfn/vpc/VPCPubRouteTable.template --parameters ParameterKey=VpcId,ParameterValue=<vpc id> ParameterKey=RouteTableName,ParameterValue=sandy-vpc-pub-sn-az-a ParameterKey=IDofIGW,ParameterValue=<id of igw>

AWS cli command to get the Outputs
aws cloudformation describe-stacks --stack-name sandy-vpc-pub-rt

3.) Details about creation of Public subnet in AZ a & b

Template to create Public subnet
VPCSubnet.template

Input Parameters:
VPC:
SubnetNameTagValue:
PublicIPOnLaunch:
CIDR:
availabilityZone:
RouteTableId:

Description: This template will create a Public Subnets

AWC cli command:
aws cloudformation create-stack --stack-name sandy-vpc-pub-sn-az-a --template-body file:///data/codebase/opstree/OpstreeCfn/vpc/VPCSubnet.template --parameters ParameterKey=VPC,ParameterValue=<vpc id> ParameterKey=SubnetNameTagValue,ParameterValue=sandy-vpc-pub-sn-az-a ParameterKey=PublicIPOnLaunch,ParameterValue=false ParameterKey=CIDR,ParameterValue=10.240.10.0/24 ParameterKey=availabilityZone,ParameterValue=ap-southeast-2a ParameterKey=RouteTableId,ParameterValue=<id of route table>

AWS cli command to get the Outputs
aws cloudformation describe-stacks --stack-name sandy-vpc-pub-sn-az-a

Both of the above steps can be performed to create subnet in az-b

4.) Details about creation of NAT Instance in AZ a & b

Template to create Public subnet
VPCNATInstance.template

Input Parameters:
KeyName:
myImageID:
PublicSubnetID:
SecurityGroupIds:
InstanceType:


Description: This template will create a NAT instance

AWC cli command:
aws cloudformation create-stack --stack-name sandy-vpc-pub-sn-az-a-nat --template-body file:///data/codebase/opstree/OpstreeCfn/vpc/VPCNATInstance.template --parameters ParameterKey=KeyName,ParameterValue=<KeyName> ParameterKey=myImageID,ParameterValue=<nat ami> ParameterKey=PublicSubnetID,ParameterValue=<id of subnet in which nat to be launched> ParameterKey=SecurityGroupIds,ParameterValue=<list of sg> ParameterKey=InstanceType,ParameterValue=t2.micro

AWS cli command to get the Outputs
aws cloudformation describe-stacks --stack-name sandy-vpc-pub-sn-az-a-nat

Both of the above steps can be performed to create nat instance in az-b

5.) Details about creation of Private RouteTable

Template to create Public Route table:
VPCPvtRouteTable.template

Input Parameters:
VpcId:
RouteTableName:
IDofNAT:

Description: This template will create a Private Route Table

AWC cli command:
aws cloudformation create-stack --stack-name sandy-vpc-pvt-rt --template-body file:///data/codebase/opstree/OpstreeCfn/vpc/VPCPvtRouteTable.template --parameters ParameterKey=VpcId,ParameterValue=<vpc id> ParameterKey=RouteTableName,ParameterValue=sandy-vpc-pub-sn-az-a ParameterKey=IDofNAT,ParameterValue=<id of nat>

AWS cli command to get the Outputs
aws cloudformation describe-stacks --stack-name sandy-vpc-pvt-rt

6.) Creating a security group to allow public ssh access

Template to create Security Group
VPCPubSshSecurityGroup.template

Input Parameters:
myVPC:
Environment:
Name:
Project:


Description: This template will create a Public SSH access security group

AWC cli command:
aws cloudformation create-stack --stack-name sandy-vpc-pub-sg-ssh --template-body file:///data/codebase/opstree/OpstreeCfn/vpc/VPCPubSshSecurityGroup.template --parameters ParameterKey=myVPC,ParameterValue=<vpc id> ParameterKey=Project,ParameterValue=sandy

AWS cli command to get the Outputs
aws cloudformation describe-stacks --stack-name sandy-vpc-pub-sg-ssh

7.) Details about creation of an Instance in AZ a & b

Template to create instance
VPCInstance.template

Input Parameters:
KeyName:
myImageID:
SubnetID:
SecurityGroupIds:
InstanceType:
Environment:
Name:
Project:


Description: This template will create a NAT instance

AWC cli command:
aws cloudformation create-stack --stack-name sandy-vpc-app-sn-az-a-bbapp --template-body file:///data/codebase/opstree/OpstreeCfn/vpc/VPCInstance.template --parameters ParameterKey=KeyName,ParameterValue=sandy-key ParameterKey=myImageID,ParameterValue= ParameterKey=SubnetID,ParameterValue=sandy-vpc-mtn-pvt-sn-az-a ParameterKey=SecurityGroupIds,ParameterValue=<sgid> ParameterKey=SecurityGroupIds,ParameterValue=<sgid> ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=Name,ParameterValue=sandy-vpc-app-sn-az-a-bbapp ParameterKey=Project,ParameterValue=bb

AWS cli command to get the Outputs
aws cloudformation describe-stacks --stack-name sandy-vpc-pub-sn-az-a-nat

Both of the above steps can be performed to create subnet in az-b
