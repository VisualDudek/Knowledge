---
description: prepare for AWS Cloud Practitioner Certificate
---

# Cloud Practitioner

## MAIN

## src

* [Prepare for Exam](https://aws.amazon.com/certification/certification-prep/)

## Linux Academy - AWS Essentials

* project Omega 2.0 [diagram](https://interactive.linuxacademy.com/diagrams/ProjectOmega2.html)
* AWS [Documentation](https://docs.aws.amazon.com/index.html)
* check other Hands-On Labs dedicated to AWS

### Dictionary

| abbreviation |  |
| :--- | :--- |
| EC2 | Virtual Server |
| RDS | provide database service |
| ARN | Amazon Resources Name |
| ISP | Internet Service Provider |
| AZ | Availability Zone |

### Free Tier Usage

* Tracking and Billing Widget, what is free and what is not and for how long

### Console

* Resources Groups
* One-click navigation \(pin\)
* Alerts -&gt; CloudWatch

### IAM Identity and Access Management

* policy
  * deny key override any other allow 
* Adding Users
  * Access type:
    * Programmatic
    * AWS Management Console access
* Roles - we do not attach Policies directly to services as EC2 we attach IAM Role

### Network

* gateway IGW - you can detach IGW from VPC, but cannot be detached from VPC while there are active AWS resources in the VPC \(such as an EC2 instance or RDS database\) 
* Route Tables \(RTs\) - set of rules, called routes, that are used to determine where network traffic is directed.
  * if IGW detach than Target will show Black Hole
  * if U add new IGW U need to add new route in Route Table

| Destination | Target |
| :--- | :--- |
| 172.31.0.0/16 | local |
| 0.0.0.0/0 | igw-41348d3a |

* Network Access Control List \(NACLs\) - act as Firewall, In/Out bound Rules, 
  * rules are evaluated based on rule \# from lowest to highest,
  * The first rule evaluated that applies to the traffic type gets immediately applied and executed regardless of the rules that come after.

{% hint style="info" %}
e.g. SSH outbound rule is on ephemeral port TPC 1024-65535 ??? 
{% endhint %}

* when you create a new NACL, all traffic is denied by default
  * a subnet can only be associated with one NACL at a time
* Subnet - not a great analogy but it works: if you think about your ISP being a network, than your home network can be considered a subnet of your ISP network.
  * cannot span AZ
  * e.g. N.Virginia Region has 6x AZ
  * subnets are implicit associated with Route Table
  * via additional Route Table\(without IGW\) we can make subnet private or public

### Availability Zones

Any AWS resources that you launch \(like EC2/RDS\) must be placed in a VPS subnet. Any given subnet must be located in an AZ. You can \(and should\) utilize multiple AZ to create redundancy in your architecture. This is what allows for High Availability and Fault Tolerant System

