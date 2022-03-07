What I have shown above is a single-region highly available 3 tier architecture. In this architecture, we have:

A single VPC with an Internet Gateway.

2 availability zones are used. Note that this is the minimum and is required for Application Load Balancer.

An internet facing ALB (Application Load Balancer) deployed across 2 AZs in the public subnets. User will access the application through this ALB..

In alternative way we can use software load balancer like Nginx and also its uses revers proxy

Presentation tier talks to the Application (Logic) Tier via this Internal ALB.

Data tier is implemented using RDS. Here we use the multi AZ setup so we have a standby instance in the second AZ. We also implement read scale out using a read replica on the second AZ.

Each Tier is secured by a security group that restricts traffic to the tier above. As such, the data tier’s security group only allows inbound traffic from the application tier’s security group.

The Application tier’s security group allows outbound only to the data tier’s security group and allows inbound only from the Internal ALB.

 The Internal ALB’s security group allows outbound only to the application tier’s security group and allows inbound only from the presentation tier’s security group.

 The presentation tier’s security group allows outbound only to the Internal ALB’s security group and allows inbound only from the internet facing ALB. The internet facing ALB allows inbound from anywhere but only on HTTP/HTTPS ports (80 and 443)..



