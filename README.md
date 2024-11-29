# DevOps_Sem.5


# Module

The .terraform/modules/modules.json file contains a list of modules used in the Terraform configuration. Each module is represented by a JSON object with the following properties:

Key: A unique identifier for the module.
Source: The source location of the module.
Dir: The directory where the module is located.
Module List
The following modules are defined in the .terraform/modules/modules.json file:

1. alb_region-1
Key: "alb_region-1"
Source: "./module/LoadBalancer"
Dir: "module/LoadBalancer"
2. alb_region-2
Key: "alb_region-2"
Source: "./module/LoadBalancer"
Dir: "module/LoadBalancer"
3. compute_region_a
Key: "compute_region_a"
Source: "./module/compute"
Dir: "module/compute"
4. compute_region_b
Key: "compute_region_b"
Source: "./module/compute"
Dir: "module/compute"
5. network_region_a
Key: "network_region_a"
Source: "./module/network"
Dir: "module/network"
6. network_region_b
Key: "network_region_b"
Source: "./module/network"
Dir: "module/network"
Note: The Key property is used to identify the module, while the Source and Dir properties specify the location of the module.

# The main.tf file is the primary entry point for the Terraform configuration. It defines the infrastructure components and their relationships.

Modules
The main.tf file uses the following modules:

1. Network Modules
module "network_region_a": Configures the network infrastructure for the us-east-1 region.
module "network_region_b": Configures the network infrastructure for the us-west-1 region.
2. Compute Modules
module "compute_region_a": Configures the compute infrastructure for the us-east-1 region.
module "compute_region_b": Configures the compute infrastructure for the us-west-1 region.
3. Security Modules
module "security_region_1": Configures the security infrastructure for the us-east-1 region.
module "security_region_2": Configures the security infrastructure for the us-west-1 region.
4. Load Balancer Modules
module "alb_region-1": Configures the load balancer infrastructure for the us-east-1 region.
module "alb_region-2": Configures the load balancer infrastructure for the us-west-1 region.
Module Configuration
Each module is configured with the following properties:

source: The source location of the module.
providers: The AWS provider configuration for the module.
cidr: The CIDR block for the network infrastructure.
region: The AWS region for the module.
subnets: The subnet configuration for the network infrastructure.
bastion_ami: The AMI ID for the bastion host.
ami_id: The AMI ID for the compute instances.
subnet_ids: The subnet IDs for the compute instances.
ec2_sg: The security group ID for the compute instances.
key_name: The key pair name for the compute instances.
alb_sg: The security group ID for the load balancer.
vpc_id: The VPC ID for the load balancer.
private_instance_ids: The instance IDs for the private instances.
public_subnet_ids: The subnet IDs for the public subnets.
Note: The source property is used to specify the location of the module, while the other properties configure the module's behavior.

# The variable.tf file is used to define input variables for a Terraform configuration. These variables can be used to customize the configuration and make it more flexible.

Variable Definitions
The variable.tf file defines the following variables:

1. regions
Type: map(object({...}))
Description: A map of region configurations, where each region is an object with cidr and subnets attributes.
2. ami_region_1
Type: string
Description: The AMI ID for the us-east-1 region.
3. ami_region_2
Type: string
Description: The AMI ID for the us-west-1 region.
4. key_name
Type: string
Description: The key pair name for the compute instances.

# The module/compute module is a Terraform module that provisions compute resources, including EC2 instances and security groups.

Module Structure
The module/compute module consists of the following files:

main.tf: The main Terraform configuration file for the module.
variables.tf: The file that defines the input variables for the module.
outputs.tf: The file that defines the output values for the module.
Input Variables
The module/compute module accepts the following input variables:

ami_id: The ID of the Amazon Machine Image (AMI) to use for the EC2 instances.
instance_type: The type of EC2 instance to use.
subnet_ids: A list of subnet IDs to launch the EC2 instances in.
security_groups: A list of security group IDs to associate with the EC2 instances.
key_name: The name of the key pair to use for the EC2 instances.
Output Values
The module/compute module exports the following output values:

instance_ids: A list of IDs of the EC2 instances provisioned by the module.
security_group_ids: A list of IDs of the security groups provisioned by the module.
Resources
The module/compute module provisions the following resources:

EC2 instances
Security groups

# The module/network module is a Terraform module that provisions network resources, including VPCs, subnets, and security groups.

Module Structure
The module/network module consists of the following files:

main.tf: The main Terraform configuration file for the module.
variables.tf: The file that defines the input variables for the module.
outputs.tf: The file that defines the output values for the module.
Input Variables
The module/network module accepts the following input variables:

cidr: The CIDR block for the VPC.
region: The AWS region to create the VPC in.
subnets: A list of subnet configurations, including the CIDR block, availability zone, and map public IP.
bastion_ami: The ID of the Amazon Machine Image (AMI) to use for the bastion host.
instance_type: The type of EC2 instance to use for the bastion host.
key_pair_name: The name of the key pair to use for the bastion host.
Output Values
The module/network module exports the following output values:

vpc_id: The ID of the VPC provisioned by the module.
subnet_ids: A list of IDs of the subnets provisioned by the module.
public_subnet_ids: A list of IDs of the public subnets provisioned by the module.
private_subnet_ids: A list of IDs of the private subnets provisioned by the module.
Resources
The module/network module provisions the following resources:

VPC
Subnets
Security groups
Bastion host

# The module/LoadBalancer module is a Terraform module that provisions an Elastic Load Balancer (ELB) and its associated resources.

Module Structure
The module/LoadBalancer module consists of the following files:

main.tf: The main Terraform configuration file for the module.
variables.tf: The file that defines the input variables for the module.
outputs.tf: The file that defines the output values for the module.
Input Variables
The module/LoadBalancer module accepts the following input variables:

vpc_id: The ID of the VPC to create the ELB in.
subnets: A list of subnet IDs to create the ELB in.
security_groups: A list of security group IDs to associate with the ELB.
instance_ids: A list of instance IDs to register with the ELB.
lb_type: The type of ELB to create (e.g. "application" or "network").
lb_name: The name of the ELB.
lb_port: The port number to use for the ELB.
Output Values
The module/LoadBalancer module exports the following output values:

lb_id: The ID of the ELB provisioned by the module.
lb_arn: The ARN of the ELB provisioned by the module.
lb_dns_name: The DNS name of the ELB provisioned by the module.
Resources
The module/LoadBalancer module provisions the following resources:

Elastic Load Balancer (ELB)
ELB listener
ELB target group

# The module/security module is a Terraform module that provisions security-related resources, including security groups, IAM roles, and IAM policies.

Module Structure
The module/security module consists of the following files:

main.tf: The main Terraform configuration file for the module.
variables.tf: The file that defines the input variables for the module.
outputs.tf: The file that defines the output values for the module.
Input Variables
The module/security module accepts the following input variables:

vpc_id: The ID of the VPC to create the security groups in.
security_group_names: A list of names for the security groups to create.
security_group_descriptions: A list of descriptions for the security groups to create.
security_group_rules: A list of rules for the security groups to create.
iam_role_name: The name of the IAM role to create.
iam_policy_name: The name of the IAM policy to create.
iam_policy_document: The document for the IAM policy to create.
Output Values
The module/security module exports the following output values:

security_group_ids: A list of IDs of the security groups provisioned by the module.
iam_role_arn: The ARN of the IAM role provisioned by the module.
iam_policy_arn: The ARN of the IAM policy provisioned by the module.
Resources
The module/security module provisions the following resources:

Security groups
IAM roles
IAM policies
