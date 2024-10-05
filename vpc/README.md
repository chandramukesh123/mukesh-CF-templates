###Cloudformation

Cloudformation is a service which is used to manage infrastructure in the aws cloud.

with this we can manage

       1. Configurations
       2. Provisions
       3. Deployments
       
**Terminology:**
    
   Template: JSON or YAML format text file
   
   Stack: when cloudformation executes a template, it will create stack.
   
   A set of related resources as a single unit is called a **stack**.
   
   
####Template Anatomy:

**YAML Format:**

     AWSTemplateFormatversion: "version date"
     Description: String
     Metadata: Template metadata
     Parameters: set of parameters
     Mappings:  set of mappings
     Conditions: set of conditions
     Transform:  set of Transforms
     Resourses: set of resources
     Outputs:  set of outputs       
       
  In above, Resourses are mandatory, Remaining all are optional.
       
**Resources Structure:**

   **YAML:**
   
         Resources:
            Logical Id:
               Type: Resources type
               properties: 
                  set of properties
       
  For every task, **logical Id** is required. we can refer this logical Id wherever we want, instead of writing static values.
       
### FTL VPC Setup using Cloudformation template

* For this FTL VPC, we require more than thousand ip's so we are taking subnet mask as **/22**.
 
* Here we are creating vpc in **one region** and subnets are in **2 different az's** in the specific region.
 
* For VPC Setup, we need to create one vpc and subnets as we require , and route tables, internet gateway, NAT Gateway, Elastic ip address, etc..
   
* In Cloudformation, multiple **resource types** will be available. By using these resource types, we can create whatever resources we want.
 
* As per requirement, we have created VPC with the cidr block **172.17.16.0/22** by using resource type **AWS::EC2::VPC**. For this vpc, dns support and dns hostnames are **enabled** and Instance tenancy is **default**.
 
* If we put instance tenancy is dedicated, we can't able to launch some AMI's in the VPC.
 
* Here we have created **public** and **private** subnets as per the requirement.

* By using **AWS::EC2::Subnet** resource type, we can create subnets.
 
* If we want to assign public ip to specific subnet, in properties we need to use the following line
 
      MapPublicIpOnLaunch: true
 
* Public subnets are
 
      subnet-perfios-16-public-a
 
      subnet-GW-perfios-16-public-a
  
      subnet-perfios-18-public-b
 
      subnet-GW-perfios-18-public-b
 
* Private subnets are
 
      subnet-web-perfios-16-private-a
 
      subnet-app-perfios-17-private-a
 
      subnet-db-perfios-17-private-a
 
      subnet-nosql-perfios-17-private-a
 
      subnet-web-perfios-18-private-b
 
      subnet-app-perfios-19-private-b
 
      subnet-db-perfios-19-private-b
 
      subnet-nosql-perfios-19-private-b
 
* While creating subnets, we need to give the vpc id , for that we can use **!Ref** intrinsic function and we can give there logical Id of vpc
  
*  We have created 2 Route tables. one is for public and another one for private.
  
*  We can create route tables by using resource type **AWS::EC2::RouteTable**
  
*  Public Route table, we need to attach with Internet Gateway. Then only public instances which are in public subnet will connect to Internet.for that we need to create **Internet gateway**.
  
* Internet gateway , we can create by using **AWS::EC2::InternetGateway**
  
*  we need to attach Internet gateway with VPC.
  
*  **AWS::EC2::VPCGatewayAttachment** resource type is used for attaching internet gateway with vpc.
  
*  **AWS::EC2::Route** resource type is used to route Public route table to Internet Gateway.
   
*  Private Route table, we need to attach with NAT Gateway. This NAT Gateway resides in public subnet and should attach with private route table. then only private instances which are in private subnet will connect to Internet.
  
*  To create NAT Gateway, we need Elastic ip. after creating Elastic Ip, we need to attach with NAT Gateway.

*  By using **AWS::EC2::EIP** , we can create Elasitc ip address.
  
*  To create NAT Gateway, **AWS::EC2::NatGateway** resource type we will use.
  
*  we can attach private route table with NAT Gateway by using **AWS::EC2::Route**. If we want we can use, otherwise no need. 
  
*  Every subnet need routing, to achieve this every subnet need to associate with routetable(public or private). 
  
*  **AWS::EC2::SubnetRouteTableAssociation** we can use this to associate subnets with respective route tables.  
  
  
  ![VPC Image](/home/mukesh/Downloads/VPC.png)
