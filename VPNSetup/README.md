###Site-to-Site VPN:

* By default, instances that you launch into an Amazon VPC can't communicate with your own (remote) network. You can enable access to your remote network from your VPC by creating an AWS **Site-to-Site VPN** connection, and configuring routing to pass traffic through the connection.

* **VPN connection:** A secure connection between your on-premises equipment and your VPCs.

* **VPN tunnel:** An encrypted link where data can pass from the customer network to or from AWS.

* **Customer gateway:** An AWS resource which provides information to AWS about your customer gateway device.


**Site-to-Site VPN Limitations:**

 A Site-to-Site VPN connection has the following limitations.

  * IPv6 traffic is not supported.

  * An AWS VPN connection does not support Path MTU Discovery.


**Componets of Site-to-Site:**

* A Site-to-Site VPN connection offers two VPN tunnels between a virtual private gateway or a transit gateway on the AWS side, and a customer gateway on the remote (on-premises) side. A Site-to-Site VPN connection offers two VPN tunnels between a virtual private gateway or a transit gateway on the AWS side, and a customer gateway on the remote (on-premises) side.

* A Site-to-Site VPN connection consists of the following components.

  * Virtual Private Gateway
  * AWS Transit Gateway
  * Customer Gateway
  * Customer Gateway Device

 *  In our project, we are using Virtual Private Gateway and Customer Gateway only.
   
**Virtual Private Gateway:**

* A virtual private gateway is the VPN concentrator on the Amazon side of the Site-to-Site VPN connection. You create a virtual private gateway and attach it to the VPC from which you want to create the Site-to-Site VPN connection.

###VPN Setup between two networks using Cloudformation:

* We need to create Customer Gateways to different ISP's which we have in our organization.

* We can create Customer Gateway by using resource type **AWS::EC2::CustomerGateway** in Cloudformation.

* We need to create Virtual Gateway and need to attach with VPC. For this, **AWS::EC2::VPNGateway** and **AWS::EC2::VPCGatewayAttachment** resource types we can use.

* Now we can create site-to-site vpn between different ISP's by using **AWS::EC2::VPNConnection** resource type and we need to give static ip addresses which is used by different ISP's by using resource type **AWS::EC2::VPNConnectionRoute**.

* In properties, we can give our static IP addresses. Like this we need to give for every site-to-site vpn connection.


![VPN Image](/home/mukesh/Downloads/VPNSetup.png)
