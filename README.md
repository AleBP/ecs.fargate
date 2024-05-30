# ecs.fargate
This AWS CloudFormation template sets up an Amazon ECS cluster with Fargate, including necessary resources like subnets, security groups, load balancers, task definitions, and auto-scaling configurations.
Parameters
These are inputs required when deploying the stack:

ECSSubnets: List of subnets for ECS clusters.
VpcId: The ID of the VPC for resources.
KeyPair: Name of an existing EC2 KeyPair for SSH access.
LoadBalancerSubnets: List of subnets for the load balancer.
Image: Docker image to use, defaulting to caddy:latest.
ServiceName: The name of the ECS service, defaulting to Service.
healthCheckType: Health check type for the ALB, defaulting to ELB.
MaxSize: Maximum number of instances in the ECS cluster, defaulting to 2.
MinSize: Minimum number of instances in the ECS cluster, defaulting to 1.
DesiredCapacity: Desired number of instances in the ECS cluster, defaulting to 1.
DNSNombre: DNS name for the application, defaulting to myweb.
ACMARN: ACM certificate ARN for the domain, defaulting to mycertificate.
HostedZoneId: Hosted Zone ID for creating the subdomain, defaulting to myhostedzone.
AutoScalingTargetValue: Target CPU utilization for auto-scaling, defaulting to 50.
ECRName: Name of the ECR repository to create, defaulting to myecr.
Resources
The resources section defines all the AWS resources needed for this setup:

ECS Cluster and Task Definition
Cluster: Creates an ECS cluster.
TaskDefinition: Defines the ECS task with specified CPU, memory, and container image. Logs are configured to be sent to CloudWatch Logs.
Security Groups
ContainerInstancesSecurityGroup: Security group for ECS instances allowing SSH (port 22), HTTP (port 80), and HTTPS (port 443) access.
LoadBalancerSecurityGroup: Security group for the load balancer allowing HTTP and HTTPS access.
Load Balancer and Listeners
ApplicationLoadBalancer: An Application Load Balancer (ALB).
ApplicationLoadBalancerListener: A listener for the ALB on port 80.
HttpListenerRule: A rule for redirecting HTTP traffic to HTTPS.
HttpsListener: A listener for HTTPS traffic on port 443.
ListenerRule: A rule to forward traffic to the null target group based on the host header.
Target Groups
NullTargetGroup: A target group used for health checks and to forward traffic.
Route53 DNS Record
MiSubdominio: A DNS A record in Route 53 for the subdomain pointing to the ALB.
ECR Repository
MyECRRepository: Creates an Amazon Elastic Container Registry (ECR) repository.
IAM Roles
ExecutionRole: An IAM role for ECS task execution.
TaskRole: An IAM role for the containers in the task.
AutoScalingRole: An IAM role for ECS auto-scaling.
ECS Service
Service: Creates an ECS service using the Fargate launch type with specified desired count, health check settings, and network configuration. It associates the service with the ALB.
CloudWatch Log Group
LogGroup: A CloudWatch log group for ECS task logs.
Auto Scaling
AutoScalingTarget: Defines a scalable target for ECS service desired count.
AutoScalingPolicy: Sets up a scaling policy to maintain the target CPU utilization.
Outputs
Cluster: Outputs the ECS cluster.
Listener: Outputs the ALB listener.
ApplicationLoadBalancerEndpoint: Outputs the DNS name of the ALB.
SecurityGroup: Outputs the security group ID for ECS instances.
This template automates the creation of a scalable ECS cluster with a Fargate launch type, incorporating networking, security, and load balancing configurations.

P.S: i used caddy as predefinited, change the image as you want
