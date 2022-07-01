# Guide to AWS CloudTrail Events  

- [Auto Scaling](#auto-scaling-cloud-trail-events)
- [CloudFormation](#cloudformation-cloudtrail-events)
- [Certificate Manager](#certificate-manager-cloudtrail-events)
- [Disable Logging (Only if you want to stop logging, Not recommended to use)](#disable-cloudtrail-logging)
- [AWS Config](#aws-config-cloudtrail-events)
- [Direct Connect](#direct-connect-cloudtrail-events)
- [EC2](#ec2-cloudtrail-events)
- [VPC](#ec2-vpc-cloudtrail-events)
- [EC2 Security Groups](#ec2-security-groups-cloudtrail-events)
- [EFS Elastic File System](#efs-cloudtrail-events)
- [Elastic Beanstalk](#elastic-beanstalk-cloudtrail-events)
- [ElastiCache](#elasticache-cloudtrail-events)
- [ELB](#elb-cloudtrail-events)
- [IAM](#iam-cloudtrail-events)
- [Redshift](#redshift-cloudtrail-events)
- [Route 53](#route-53-cloudtrail-events)
- [S3](#s3-cloudtrail-events)
- [WAF](#waf-cloudtrail-events)

--

## Auto Scaling Cloud Trail Events  

AWS Auto Scaling emits a handful of events that a business may want to keep an eye on, mostly relating to load balancers and policies.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AttachLoadBalancers   | A load balancer has been attached to an auto scaling group. |
| DetachLoadBalancers   | A load balancer has been detached from an auto scaling group. |
| PutScalingPolicy      | A policy has been updated for an Application Auto Scaling scalable target. |
| DeletePolicy          | A scaling policy has been deleted. In the case of a “target tracking scaling policy” this will mean that any associated CloudWatch alarms will have been deleted, but this will not be the case of “step scaling policies” or “simple scaling policies”. |
| TerminateInstanceInAutoScalingGroup | An instance inside an auto scaling group has been terminated. |

--

## CloudFormation CloudTrail Events  

CloudTrail events for CloudFormation that should be observed are primarily around the creation, changing and removal of CloudFormation stacks.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| CancelUpdateStack     | A stack update has been cancelled. In this case, your stack will end up matching its previous configuration. |
| CreateStack           | A stack has been created using CloudFormation. If you want more information about the stack itself, you can use the DescribeStacks API to retrieve more detailed metadata. |
| DeleteStack           | A stack has been deleted. In this case there is no stack to describe in the DescribeStacks API so it won’t return the details of this particular stack. |
| UpdateStack           | A stack has been updated. Once again, you can use the DescribeStack API to see the current configuration of the stack. |

--

## Certificate Manager CloudTrail Events  

These events are fairly straightforward and limited.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| DeleteCertificate     | An Amazon Certificate Manager (ACM) Certificate has been deleted along with its associated private key. |
| RequestCertificate    | An ACM certificate has been requested for use with other services. |
| ResendValidationEmail | An email has been resent that requests domain ownership validation. |

--

## Disable CloudTrail Logging  

**NOTE:** Not recommended  


This is a key event.. and a little bit meta! If you’re planning on logging CloudTrail events and having oversight of your AWS environment, the last thing you want is to stop logging those events.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| StopLogging           | CloudTrail has stopped recording CloudTrail Events. This is a significant red flag and should almost always be avoided. |

--

## AWS Config CloudTrail Events  

Many organizations use Config as key tool in their arsenal for compliance and monitoring of AWS resources. Changes to Config and Config Rules may have serious implications for an environment’s governance so should be monitored carefully.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| DeleteConfigRule      | A Config Rule has been deleted along with all of its evaluation results. This might be of particular concern as it could disrupt your compliance oversight. |
| DeleteConfigurationRecorder | A configuration recorder has been deleted which also means that resource configuration changes are no longer being recorded which may be of concern. You can still access older information with the GetResourceConfigHistory action via the API. |
| DeleteDeliveryChannel | The Delivery Channel for a Config Rule has been deleted. This would have to have followed a StopConfigurationRecorder action in order to have taken place which means you may wish to review any StopConfigurationRecorder actions too. |
| DeleteEvaluationResults | The evaluation results for a Config Rule have been deleted. This can be benign in the case that a user simply want to re-evaluate a rule but can also be used to cover up for failed rules so should be taken seriously. |
| PutConfigurationRecorder | A new configuration recorder has been created, it may also indicate than the configuration recorder has had its role ARN or recordingGroup updated. |
| PutConfigRule         | A Config Rule has been created or updated. |
| PutDeliveryChannel    | A Delivery Channel has been created to deliver Config Rule information to S3 or SNS. |
| PutEvaluations        | A Lambda function has been invoked by a Config Rule and delivered evaluation results. |
| StartConfigRulesEvaluation | An evaluation has been run for the set of Config Rules against the last known configuration state of resources. |
| StartConfigurationRecorder | Configurations are being recorded for a designated set of resources. |
| StopConfigurationRecorder  | Configurations have stopped being recorded for a designated set of resources. If this is unexpected then it probably merits further investigation given the risks associated with no longer recording. |

--

## Direct Connect CloudTrail Events  

These events are mostly around creation, update and deletion of connections between a location outside of AWS (i.e. a datacenter or office) and AWS itself.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AllocateHostedConnection | A hosted connection has been created on an interconnect or on a link aggregation group of interconnects. |
| AllocatePrivateVirtualInterface | A private virtual interface has been connected. This won’t handle traffic until it has been confirmed at which point you should see a ConfirmPrivateVirtualInterface event. |
| AllocatePublicVirtualInterface | A public virtual interface has been connected. This won’t handle traffic until it has been confirmed at which point you should see a ConfirmPublicVirtualInterface event. |
| AssociateConnectionWithLag | A connection has been associated with a link aggregation group. |
| AssociateHostedConnection | A hosted connection, along with its virtual interfaces, has been associated with a link aggregation group. |
| AssociateVirtualInterface | A virtual interface has been associated with a link aggregation group. Connectivity to AWS will have been temporarily interrupted during the process. |
| ConfirmConnection | | A hosted connection has been created and confirmed on an interconnect. |
| ConfirmPrivateVirtualInterface | A private virtual interface has been created by another AWS account, and accepted. |
| ConfirmPublicVirtualInterface | A public virtual interface has been created by another AWS account, and accepted. |
| CreateConnection | A connection has been created between the network and a Direct Connect location. |
| CreateInterconnect | An interconnect has been created between an AWS Direct Connect Partner’s network and a Direct Connect location. |
| CreateLag | A link aggregation group has been created. |
| CreatePrivateVirtualInterface | A private virtual interface has been created which can then be connected to a Direct Connect gateway of a Virtual Private Gateway. |
| CreatePublicVirtualInterface | A public virtual interface has been created which can send traffic to public AWS services. |
| DeleteConnection | A connection has been deleted. |
| DeleteInterconnect | An interconnection has been deleted. |
| DeleteLag | A link aggregation group has been deleted. |
| DeleteVirtualInterface | A virtual interface has been deleted. |
| DisassociateConnectionFromLag | A connection has been disassociated from a link aggregation group. It has then become a standalone connection. Had it been fully deleted you would have also seen a DeleteConnection event. |
| UpdateLag | A link aggregation group has been updated – this may include its name or its minimum number of connections. |

--

## EC2 CloudTrail Events  

A handful of events that provide information when the state of an instance has been changed. The associated metadata ought to provide insight into the region, who made the change (e.g. the start or the stop), when it was made and more. For keeping an eye on EC2, organizations will often use a combination of CloudTrail and CloudWatch to keep an eye on events and performance respectively.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| RunInstances | An Instance has been launched. From the associated metadata you’ll be able to determine who the owner is , what regions the resources are in, the InstanceType and more. |
| StartInstances | An instance has been started. Similar metadata to RunInstances will give you an insight into more detail. |
| StopInstances | An instance has been stopped. Similar to StartInstances and RunInstances. |
| TerminateInstances | An instance has been terminated – as with the above 3, there is plenty of metadata to provide further insight. |

--

## EC2 VPC CloudTrail Events  

There are a number of important events in this category that, when combined, can be used to paint a fairly detailed picture of the lifecycle of a VPC.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AssociateIamInstanceProfile | An IAM instance profile has been associated with an instance. Only one instance profile can be associated with an instance and it doesn’t matter if that instance was running or stopped. |
| AssociateAddress | An Elastic IP address has been associated with an instance or a network interface. |
| AssociateRouteTable | A subnet has been associate with a route table in the same VPC. |
| AssociateSubnetCidrBlock | A CIDR block has been associated with a subnet. |
| AssociateVpcCidrBlock | A CIDR block has been associated with a VPC. |
| AttachClassicLinkVpc | An EC2-Classic instance has been linked to a ClassicLink-enabled VPC through a VPC’s security group. |
| AttachInternetGateway | An internet gateway has been attached to a VPC, connecting the VPC to the internet. |
| AttachNetworkInterface | A network interface has been attached to an instance. |
| AllocateAddress | An elastic IP address has been allocated to an AWS account in preparation for association with an instance or network interface, see Associate Address. |
| AssignPrivateIpAddresses | A secondary IP address has been assigned to a network interface. |
| AttachVpnGateway | A virtual private gateway has been attached to a VPC.|
| CreateNatGateway | A NAT gateway has been crated. A network interface with a private IP address has been created in the subnet, the private IP address having been taken from the IP address range of the subnet. |
| CreateKeyPair | A key pair has been created. |
| CreateNetworkAcl | A network ACL has been created inside a VPC. |
| CreateNetworkAclEntry | A new rule has been created in a network ACL. |
| CreateNetworkInterface | A network interface has been created in a subnet. |
| CreateRoute | A route has been created in a rout table inside a VPC. |
| CreateRouteTable | A route table has been created for a VPC. |
| CreateSecurityGroup | A security group has been created. |
| CreateVpc | A VPC has been created. |
| CreateVpcEndpoint | A VPC endpoint has been created, enabling a private connection between the VPC and another service. |
| CreateVpcPeeringConnection | A VPC connection (connecting two VPCs) has been requested. |
| CreateVpnConnection | A VPC connection between a virtual private gateway and a VPN customer gateway has been created. |
| CreateVpnConnectionRoute | A static route has been created for a VPN connection between a virtual private gateway and a VPN customer gateway. |
| CreateVpnGateway | A virtual private gateway has been created. |
| DeleteCustomerGateway | A customer gateway has been deleted. The VPN connection will have been deleted beforehand (see DeleteVpnConnection). |
| DeleteDhcpOptions | A set of DHCP Options have been deleted. This will have been preceded by a dissociation of those DHCP options. |
| DeleteEgressOnlyInternetGateway | An egress-only internet gateway has been deleted. |
| DeleteInternetGateway | An internet gateway has been deleted. The gateway will have been detached beforehand (see DetachInternetGateway). |
| DeleteKeyPair | A key pair has been deleted by removing the public key from the EC2. |
| DeleteNatGateway | A NAT gateway has been deleted which means the Elastic IP address will have been dissociated but not released from the account. No NAT gateway routes in the route table were necessarily deleted. |
| DeleteNetworkAcl | A network ACL has been deleted. |
| DeleteNetworkAclEntry | An ingress or egress rule has been deleted from a network ACL. |
| DeleteNetworkInterface | A network interface has been deleted. It would have been detached initially (see DetachNetworkInterface). |
| DeleteRoute | A route has been deleted from a route table. |
| DeleteRouteTable | A route table has been deleted after it was disassociated (see DisassociateRouteTable). |
| DeleteSecurityGroup | A security group was deleted. |
| DeleteVpcEndpoints | One or more VPC endpoints have been deleted. This also means that endpoint routes in the route tables may have been deleted. |
| DeleteVpcPeeringConnection | A VPC peering connection has been deleted. Depending on the state of the connection, it may have been deleted by the owner of the requester VPC or the owner of the accepter VPC. |
| DeleteVpnConnection | A VPN connection has been deleted. |
| DeleteVpnConnectionRoute | A static route for a VPN connection between a virtual private gateway and a VPN customer gateway has been deleted. |
| DeleteVpnGateway | A virtual private gateway has been deleted. |
| DetachClassicLinkVpc | An EC2-classic instance has unlinked from a VPC. This may be because the instance was stopped. Once it is unlinked it is disassociated with the VPC security groups. |
| DetachInternetGateway | An internet gateway has been detached from a VPC, severing its connection to the internet. |
| DetachNetworkInterface | A network interface has been detached from an instance. |
| DetachVolume | An EBS volume has been detached from an instance. Detached volumes can run up significant AWS costs, but you can use GorillaStack to detect and delete detached volumes automatically. |
| DetachVpnGateway | A virtual private gateway has been detached from a VPC. |
| DisableVgwRoutePropagation | A virtual private gateway has been disabled from propagating routes to a route table in the VPC. |
| DisableVpcClassicLink | A classic link for a VPC has been disabled. |
| DisassociateAddress | An elastic IP address has been disassociated from an instance or network. |
| DisassociateIamInstanceProfile | An IAM instance profile has been disassociated from an instance. That instance may have been running or it may have been stopped. |
| DisassociateRouteTable | A subnet has been disassociated from a route table meaning the subnet will now use the VPC’s main route table. |
| DisassociateSubnetCidrBlock | A CIDR block has been disassociated from a subnet. |
| DisassociateVpcCidrBlock | A CIDR block has been disassociated from a VPC. |
| EnableVgwRoutePropagation | A virtual private gateway has been enabled to propagate routes to a route table of a VPC. |
| EnableVolumeIO | I/O operations for a volume have been enabled. |
| EnableVpcClassicLink | A VPC for a ClassicLink has been enabled, usually to allow EC2-Classic instance to link to ClassicLink-enabled VPC, allowing communication over private IP addresses. |

--

## EC2 Security Groups CloudTrail Events  

These events report on any changes to ingress and egress rules.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AuthorizeSecurityGroupEgress | An egress rule has been added to a security group for use with a VPC. |
| AuthorizeSecurityGroupIngress | An ingress rule has been added to a security group, permitting instances to receive traffic from certain CIDR address ranges or from other instances associated with certain destination security groups. |
| RevokeSecurityGroupEgress | An egress rule has been removed from a security group for a VPC. |
| RevokeSecurityGroupIngress | An ingress rule has been removed from a security group for a VPC. |

--

## EFS CloudTrail Events  

There are a few EFS events that relate to the creation modification and deletion of AWS Elastic File System.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| CreateFileSystem | A new file system has been created. |
| CreateMountTarget | A new mount target has been created for a file system. |
| DeleteFileSystem | A file system has been deleted. If this is unexpected then this warrants further investigation as the contents will have been permanently lost. |
| DeleteMountTarget | A mount target has been deleted. |
| ModifyMountTargetSecurityGroups | A set of security groups for a mount target have been modified. |

--

## Elastic Beanstalk CloudTrail Events  

Elastic Beanstalk events can be monitored and inspected for clues as to why an application may be failing.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AbortEnvironmentUpdate | An in-progress environment configuration update or application version deployment has been cancelled. |
| CreateApplication | An application has been created. |
| CreateApplicationVersion | An application version has been created, you can find the details of the specific application in the metadata. |
| CreateConfigurationTemplate | A template has been created which can then be used to deploy multiple versions of the specified application. |
| CreateEnvironment | An Elastic Beanstalk environment has been created. |
| CreateStorageLocation | An S3 bucket has been created. The S3 bucket is used to store files and data associated with the Elastic Beanstalk. You’ll always see this event the first time an environment is created in a new region. |
| DeleteApplication | An application has been deleted although its versions will still remain in S3. |
| DeleteApplicationVersion | An application version has been deleted. |
| DeleteConfigurationTemplate | A configuration template has been deleted. |
| DeleteEnvironmentConfiguration | A configuration has been deleted. |
| RebuildEnvironment | An Elastic Beanstalk environment has been deleted, recreated and subsequently restarted. “Have you tried turning it off and back on again?”. |
| RestartAppServer | An application container server has been restarted. |
| SwapEnvironmentCNAMEs | The CNAMEs of 2 environments have been swapped. |
| TerminateEnvironment | An Elastic Beanstalk environment has been terminated. |
| UpdateApplication | An application has been updated. |
| UpdateApplicationVersion | An application version has been updated. |
| UpdateConfigurationTemplate | A configuration template has been updated. |
| UpdateEnvironment | An Elastic Beanstalk environment has been updated. |

--

## ElastiCache CloudTrail Events  

These events are used to monitor changes to cache security groups.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AuthorizeCacheSecurityGroupIngress | Network ingress to a cache security group has been permitted. |
| CreateCacheSecurityGroup | A cache security group has been created to control access to one or more clusters. |
| DeleteCacheSecurityGroup | A cache security group has been deleted. In order to have been deleted it will not have been associated with any clusters at the time. |
| RevokeCacheSecurityGroupIngress | Ingress has been revoked from a cache security group. |

--

## ELB CloudTrail Events  

These events can be tracked and reviewed to monitor changes to load balancers. Given their relative breadth, they can put together quite a detailed picture of the lifecycle of a load balancer.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| ApplySecurityGroupsToLoadBalancer | A security group has been associated with a load balancer inside a VPC. |
| CreateListener | A listener has been created for the Elastic Load Balancer. |
| CreateLoadBalancer | A load balancer has been created. |
| CreateLoadBalancerPolicy | A policy has been created for the load balancer (only applies to Classic Load Balancer). |
| CreateRule | A rule has been created for a listener that’s associated with an Application Load Balancer. You can use DescribeRules to view all current rules. |
| CreateTargetGroup | A target group has been created. |
| DeleteListener | A listener has been deleted. This might have happened automatically when the load balancer to which it was attached was deleted. |
| DeleteLoadBalancer | A load balancer has been deleted along with its attached listeners (see DeleteListener). |
| DeleteLoadBalancerListeners | A listener has been deleted. Similar to DeleteListener but for Classic Load Balancer. |
| DeleteRule | A rule has been deleted. |
| DeleteTargetGroup | A target group has been deleted. |
| DeregisterTargets | A target has been deregistered. This means the target is no longer receiving traffic from the load balancer. |
| ModifyListener | Properties from a listener have been modified. |
| ModifyLoadBalancerAttributes | Attributes from either an Application Load Balancer or Network Load Balancer have been modified. |
| ModifyRule | A rule has been modified. |
| ModifyTargetGroup | The health checks being used to evaluate the health state of targets in a group have been modified. |
| ModifyTargetGroupAttributes | Attributes of a target group have been modified. |
| RegisterTargets | A new target has been registered with a target group. |
| RemoveTags | Tags have been removed from an ELB resource. |
| SetSecurityGroups | A security group has been associated with a load balancer. |

--

## IAM CloudTrail Events  

These events are key to monitoring and managing who has access to an AWS environment. Businesses will want to keep a key eye on this to review and receive alerts for changes to permissions that may allow users to access and update more infrastructure than ought to be permitted. While there are a lot here, they should be taken seriously and some may even merit real time monitoring with our Real Time Events product to preempt access issues before they take place.  


| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AddClientIDToOpenIDConnectProvider | A client ID has been registered for an IAM OpenID Connect provider resource. |
| AddRoleToInstanceProfile | An IAM role has been added to an instance profile. |
| AddUserToGroup | A user has been added to a group. |
| AttachGroupPolicy | A managed policy has been added to an IAM group. |
| AttachRolePolicy | A managed policy has been added to an IAM role. |
| AttachUserPolicy | A managed policy has been added to an IAM user. |
| ChangePassword | A password for an IAM user has been changed. |
| ConsoleLogin | A user has signed into AWS Management Console. That user could be an account owner, a federated user or an IAM user. |
| CreateAccessKey | A new AWS secret access key and access key ID has been created. |
| CreateAccountAlias | An alias has been created for an AWS account. |
| CreateGroup | A new group has been created. |
| CreateInstanceProfile | A new instance profile has been created. |
| CreateLoginProfile | A new password has been created for a user to access AWS services through the management console. |
| CreateOpenIDConnectProvider | An IAM entity has been created. It describes an identity provider that supports OpenID Connect. |
| CreatePolicy | A new managed policy has been created for an AWS account. |
| CreatePolicyVersion | A new version of a manged policy has been created. |
| CreateRole | A new role for an AWS account has been created. |
| CreateSAMLProvider | An IAM resource has been created. It describes an identity provider for SAML. |
| CreateUser | A new IAM user has been created for an AWS account. |
| CreateVirtualMFADevice | A new virtual MFA device has been created for the AWS account. |
| DeactivateMFADevice | An MFA device has been deactivated and its association has been removed from a user. |
| DeleteAccessKey | An access key pair for an IAM user has been deleted. |
| DeleteAccountAlias | An AWS account alias has been deleted. |
| DeleteAccountPasswordPolicy | A password policy for an account has been deleted. |
| DeleteGroup | An IAM group has been deleted. The group won’t have contained any users or policies at time of deletion. |
| DeleteGroupPolicy | An inline policy for an IAM group has been deleted. |
| DeleteInstanceProfile | An instance profile has been deleted. The instance will not have had an associated rule at time of deletion. |
| DeleteLoginProfile | A password for an IAM user has been deleted thus removing that user’s ability to access services through the console. |
| DeleteOpenIDConnectProvider | An OpenID Connect identity provider has been deleted. |
| DeletePolicy | A managed policy has been deleted. To be deleted it will have been detached from all users, groups and roles already. |
| DeletePolicyVersion | A version of a policy has been deleted. |
| DeleteRole | A role has been deleted. The role will not have had any policies attached if it was able to be deleted. |
| DeleteRolePolicy | An inline policy for an IAM role has been deleted. |
| DeleteSAMLProvider | A SAML provider resource has been deleted. |
| DeleteServerCertificate | A server certificate has been deleted. |
| DeleteSigningCertificate | A signing certificate has been deleted. |
| DeleteSSHPublicKey | An SSH public key has been deleted. |
| DeleteUser | A user has been deleted. |
| DeleteUserPolicy | An inline policy for an IAM user has been deleted. |
| DeleteVirtualMFADevice | A virtual MFA device has been deleted. |
| DetachGroupPolicy | A managed policy has been removed from an IAM group. |
| DetachRolePolicy | A managed policy has been removed from a role. |
| DetachUserPolicy | A managed policy has been removed from a user. |
| EnableMFADevice | An MFA device has been enabled. |
| PutGroupPolicy | A policy for an IAM group has been added or updated. |
| PutRolePolicy | A policy for an IAM role has been added or updated. |
| PutUserPolicy | A policy for an IAM user has been added or updated. |
| RemoveClientIDFromOpenIDConnectProvider | A client ID has been removed from an IAM OpenID Connect provider resource object. |
| RemoveRoleFromInstanceProfile | An IAM role has been removed from an EC2 instance profile. |
| RemoveUserFromGroup | A user has been removed from an IAM group. |
| ResyncMFADevice | An MFA device has been synced with an IAM resource object. |
| SetDefaultPolicyVersion | A version of a policy has been set as a default. This can apply to users, groups and roles. To find specifics, use the ListEntitiesForPolicy API. |
| UpdateAccessKey | An access key status has been updated. This will result in it becoming either Active or Inactive depending on its previous state. |
| UpdateAccountPasswordPolicy | The password policy settings for an AWS account have been updated. |
| UpdateAssumeRolePolicy | The policy for an IAM entity that dictates its permission to assume a role has been updated. |
| UpdateGroup | The name or path of an IAM group has been updated. |
| UpdateLoginProfile | A password for an IAM user has been changed. |
| UpdateOpenIDConnectProviderThumbprint | The list of server certificate thumbprints associated with an OpenID Connect provider has been replaced. |
| UpdateSAMLProvider | The metadata document for a SAML provider resource object has been updated. |
| UpdateServerCertificate | The name or path of a server certificate has been updated. |
| UpdateSigningCertificate | The status of a user signing certificate has been updated, render it it either “active” or “disabled”. |
| UpdateSSHPublicKey | The status of an SSH public key has been updated, render it it either “active” or “inactive”. |
| UpdateUser | The name or path of a user has been updated. |
| UploadServerCertificate | A server certificate entity for the AWS account has been uploaded. This will include a public key certificate, a private key and possibly a certificate chain. |
| UploadSigningCertificate | A X.509 signing certificated has been uploaded and associated with an IAM user. |
| UploadSSHPublicKey | A public key has been uploaded and associated with an IAM user. |

--

## Redshift CloudTrail Events  

These events are monitored and review to keep an eye on permissions related to Redshift access.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AuthorizeSnapshotAccess | An account has been authorized to restore a Redshift snapshot. |
| RevokeSnapshotAccess | An account can no longer restore a Redshift snapshot. |
| RotateEncryptionKey | Encryption keys for a cluster have been rotated. |
| AuthorizeClusterSecurityGroupIngress | An inbound ingress rule has been added to a Redshift security group. |
| CreateClusterSecurityGroup | A new Redshift security group has been created. |
| DeleteClusterSecurityGroup | A Redshift security group has been deleted. |
| RevokeClusterSecurityGroupIngress | An ingress rule in a Redshift security group has been revoked. |

--

## RDS CloudTrail Events  

As one of the more popular databases available inside AWS, RDS emits a number of events that warrant tracking. This is a fairly comprehensive list and paints a picture of the DB lifecycle as well as security events relating to DB access.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| AuthorizeDBSecurityGroupIngress | Ingress for a DBSecurityGroup has been enabled either via EC2/Security groups or IP ranges. |
| CreateDBCluster | A new Amazon Aurora DB cluster has been created. |
| CreateDBClusterParameterGroup | A new DB cluster parameter group has been created. These parameters can then be applied to all the instances in a cluster. |
| CreateDBClusterSnapshot | A snapshot of the cluster has been created. |
| CreateDBInstance | A new RDS DB instance has been created. |
| CreateDBInstanceReadReplica | An instance has been created to act as a Read Replica for another instance. The source instance may have been running MySQL, MariaDB, Oracle or PostgreSQL – you can get more information here. |
| CreateDBParameterGroup | A new DB parameter group has been created. |
| CreateDBSecurityGroup | A new DB security group has been created, controlling access to a DB instance. |
| CreateDBSnapshot | A snapshot of a DB has been created. |
| CreateDBSubnetGroup | A new DB subnet group has been created. |
| CreateOptionGroup | A new option group has been created. Option groups are used to specify which features can be used on an instance. |
| DeleteDBCluster | A DB cluster has been deleted. |
| DeleteDBClusterParameterGroup | A DB cluster parameter group has been deleted. If it was deleted it means that it wasn’t associated with any DB clusters at the time of deletion. |
| DeleteDBClusterSnapshot | A DB cluster snapshot has been deleted. |
| DeleteDBInstance | A DB instance has been deleted. Be careful, if this has happened it means that all automated backups for that instance were also deleted. Manual snapshots are retained, so you still have recovery options. |
| DeleteDBParameterGroup | A DM parameter group has been deleted. Similar to DeleteDBClusterParameterGroup, if it was deleted it means it wasn’t associated with any instances at the time. |
| DeleteDBSecurityGroup | A security group was deleted. |
| DeleteDBSnapshot | A snapshot was deleted. |
| DeleteDBSubnetGroup | A subnet group was deleted. |
| DeleteOptionGroup | An option group was deleted. |
| FailoverDBCluster | There was a failover for a DB cluster which means its likely that your primary instance failed and it merits investigation. |
| ModifyDBCluster | A setting for an Aurora db cluster was modified. |
| ModifyDBClusterParameterGroup | Up to 20 parameters of a DB cluster parameter group were modified. |
| ModifyDBInstance | A DB instance was modified. |
| ModifyDBParameterGroup | Up to 20 parameters of a DB parameter group were modified. |
| ModifyDBSnapshotAttribute | A manual DB snapshot had one or more of its attributes or values modified. |
| ModifyDBSubnetGroup | A DB subnet group was modified. |
| ModifyOptionGroup | An option group was modified. |
| PromoteReadReplica | A Read Replica instance became a standalone instance. |
| RebootDBInstance | An instance was rebooted. |
| ResetDBClusterParameterGroup | A DB cluster parameter group had its parameters reset to its default values. |
| ResetDBParameterGroup | A DB parameter group had its parameters reset to its default values. |
| RestoreDBClusterFromSnapshot | A DB cluster has been created from a DB snapshot or a DB cluster snapshot. Its important to note that it will launch with the default security group so if that’s not what you want or expected, you should make the appropriate changes. |
| RestoreDBClusterToPointInTime | A cluster has been restored from back to a given time. Similar to RestoreDBClusterFromSnapshot, the cluster will have been created with the default DB security group. |
| RestoreDBInstanceFromDBSnapshot | A new DB instance has been created from a snapshot. |
| RestoreDBInstanceToPointInTime | A DB instance has been been restored from back to a given time. |
| RevokeDBSecurityGroupIngress | Ingress for previously authorized EC2/VPC security groups or IP ranges has been revoked. |

--

## Route 53 CloudTrail Events  

A couple of important events that relate to the management and monitoring of DNS.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| ChangeResourceRecordSets | A record set that contains DNS information for a domain or subdomain has been created, changed or deleted. |
| DeleteHealthCheck     | A health check for Route 53 has been deleted. |

--

## S3 CloudTrail Events  

These CloudTrail events for S3 should be taken seriously as they can be significant indicators of changes in permission for users and the public to access files.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| CreateBucket | A new S3 bucket has been created. |
| DeleteBucket | An S3 bucket has been deleted. |
| DeleteBucketCors | The cors configuration information for a bucket has been deleted. |
| DeleteBucketLifecycle | The lifecycle configuration from a bucket has been deleted. |
| DeleteBucketPolicy | The policy of an S3 bucket has been deleted. |
| DeleteBucketReplication | The replication configuration has been deleted from a bucket. |
| DeleteBucketTagging | Tags have been deleted from a bucket. |
| PutBucketAcl | New permissions have been set for a bucket. |
| PutBucketCors | A new cors configuration has been set for a bucket. |
| PutBucketLifecycle | A lifecycle for a bucket has either been created or has replaced one that was there already. |
| PutBucketLogging | Logging parameter for the bucket have been updated or changed. |
| PutBucketNotification | Notifications have been defined, replaced or removed for an S3 bucket. |
| PutBucketPolicy | A policy on the bucket has been updated or replaced. |
| PutBucketReplication | A replication configuration has been created or updated for an S3 bucket. |
| PutBucketRequestPayment | There has been an update to who pays for download from the S3 bucket (i.e. the downloader or the bucket owner). |
| PutBucketTagging | Tags for an S3 bucket have been created or updated. |
| PutBucketVersioning | The versioning of an S3 bucket has been updated. It will be either Enabled or Suspended. |
| PutBucketWebsite | The website specified in the website subresource has been updated. |

--

## WAF CloudTrail Events  

As with Config, changes to WAF can be indicative of changes to an environments security posture. Events emitted by WAF should be monitored to ensure that WAF’s configuration is compliant.  

| CloudTrail Event Name | Description |
| --------------------- | ----------- |
| CreateByteMatchSet | A ByteMatchSet has been created. |
| CreateGeoMatchSet | A GeoMatchSet has been created to specify which countries web requested are allowed of blocked from. |
| CreateIPSet | A IPSet has been created. This is used to specify which web requested are allowed based on IP address. |
| CreateRateBasedRule | A RateBasedRule has been created to specify the maximum number of requests that WAF allows from one IP address in a 5 minute period. |
| CreateRegexMatchSet | A RegexMatchSet has been created. |
| CreateRegexPatternSet | A RegexPatternSet has been created. |
| CreateRule | A rule new rule has been created to identify which requests to block. |
| CreateSizeConstraintSet | A SizeConstraintSet has been created which tells WAF to check for lengths of query strings or similar. |
| CreateSqlInjectionMatchSet | A SqlINjectionMatchSet has been created which tells WAF which types of SQL code to block or allow in web requests. |
| CreateWebACL | A WebACL has been created, telling WAF which CloudFront web requests to block or allow. |
| CreateXssMatchSet | An XssMatchSet has been created which tells WAF to block or allow requests that contain cross-site scripting attacks in web requests. |
| DeleteByteMatchSet | A ByteMatchSet has been deleted. |
| DeleteGeoMatchSet | A GeoMatchSet has been deleted. |
| DeleteIPSet | An IPSet has been deleted. |
| DeleteRateBasedRule | A RateBasedRule has been deleted. |
| DeleteRegexMatchSet | A RegexMatchSet has been deleted. |
| DeleteRegexPatternSet | A RegexPatternSet has been deleted. |
| DeleteRule | A rule has been deleted. |
| DeleteSizeConstraintSet | A SizeConstraintSet has been deleted. |
| DeleteSqlInjectionMatchSet | A SqlINjectionMatchSet has been deleted. |
| DeleteWebACL | A WebACL has been deleted. |
| DeleteXssMatchSet | An XssMatchSet has been deleted. |
| UpdateByteMatchSet | A ByteMatchTuple object in a ByteMatchSet have been inserted or deleted. |
| UpdateGeoMatchSet | A GeoMatchConstraint object in a GeoMatchSet has been inserted or deleted. |
| UpdateIPSet | An IPSetDescriptor in an IPSet has been inserted or deleted. |
| UpdateRateBasedRule | A Predicate object has been inserted or deleted, resulting in an update to the RateLimit in the rule. |
| UpdateRegexMatchSet | A RegexMatchTuple object in a RegexMatchSet has been inserted or deleted. |
| UpdateRegexPatternSet | A RegexPatternString‘ object in a RegexPatternSet has been inserted or deleted. |
| UpdateRule | A Predicate object in a Rule has been deleted or inserted gd. |
| UpdateSizeConstraintSet | A SizeConstraint object in a SizeConstraintSet has been inserted or deleted. |
| UpdateSqlInjectionMatchSet | A SqlInjectionMatchTuple in a SqlInjectionMatchSet has been inserted or deleted. |
| UpdateWebACL | An ActivatedRule object in a WebACL has been inserted or deleted. |
| UpdateXssMatchSet | An XssMatchTuple object in an XssMatchSet has been inserted or deleted. |

--
