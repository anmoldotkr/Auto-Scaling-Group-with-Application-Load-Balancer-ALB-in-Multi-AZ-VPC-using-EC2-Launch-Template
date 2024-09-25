# Auto Scaling Group with Application Load Balancer in Multi-AZ VPC

This AWS CloudFormation template creates an Auto Scaling Group (ASG) with an Application Load Balancer (ALB) in a multi-AZ Virtual Private Cloud (VPC) setup. It uses an EC2 Launch Template to define the configuration of the EC2 instances. The instances run a simple Apache web server.

## Features
- **Virtual Private Cloud (VPC)**: A custom VPC with public subnets in three Availability Zones (AZs).
- **Application Load Balancer (ALB)**: Distributes traffic across EC2 instances in different subnets.
- **Auto Scaling Group (ASG)**: Automatically scales EC2 instances to handle traffic load based on demand.
- **EC2 Launch Template**: Used to define the instance type, AMI, and user data (for Apache installation).
- **Security Groups**: Configured for the ALB and EC2 instances to control inbound and outbound traffic.

## Template Structure

### Parameters
- **InstanceType**: Specifies the EC2 instance type (default: `t2.micro`).
  
### Resources
- **VPC**: A custom VPC with three public subnets in different AZs.
- **Subnets**: Three subnets, each in a different AZ, for high availability.
- **Internet Gateway**: To provide internet access for the VPC.
- **Route Table**: A public route table with a route to the internet.
- **Security Groups**: 
  - ALB security group allowing inbound HTTP traffic on port 80.
  - EC2 security group allowing inbound HTTP traffic from the ALB.
- **Application Load Balancer**: Internet-facing ALB distributing traffic to the EC2 instances.
- **Target Group**: Defines the target for the ALB, with health checks to ensure healthy instances.
- **Auto Scaling Group**: Scales EC2 instances between 1 and 3 based on load.
- **Launch Template**: Defines the EC2 instance configuration, including Apache installation.

### Outputs
- **ApplicationLoadBalancerDNS**: DNS name of the Application Load Balancer.
- **Subnet1Id, Subnet2Id, Subnet3Id**: IDs of the subnets created.
- **TargetGroupARN**: ARN of the target group for the ALB.

## How to Use

### Prerequisites
- **AWS Account**: You must have an AWS account to create the resources.
- **AWS CLI or AWS Management Console**: To deploy the CloudFormation stack.

### Steps
1. Clone this repository or download the template file.
2. Navigate to the AWS Management Console or use the AWS CLI to deploy the stack.
3. Provide the necessary parameters (e.g., EC2 instance type) when creating the stack.
4. Once the stack is created, you can access the public DNS of the Application Load Balancer to see the Apache server running.

### Example Command (AWS CLI)

```bash
aws cloudformation create-stack \
  --stack-name MyASGWithALBStack \
  --template-body file://template.yaml \
  --parameters ParameterKey=InstanceType,ParameterValue=t2.micro \
  --capabilities CAPABILITY_IAM
