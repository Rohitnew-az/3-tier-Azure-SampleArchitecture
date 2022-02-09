Problem Statement

A three-tier architecture is a software architecture pattern where the application is broken down into three logical tiers: the presentation layer, the business logic layer and the data storage layer.

This article outlines a set of proven practices for running virtual machines (VMs) for an application with a 3-tier architecture.

There are variations of N-tier architectures. For the most part, the differences shouldn't matter for the purposes of these recommendations. This article assumes a typical 3-tier web app:

Web tier. Handles incoming HTTP requests. Responses are returned through this tier.

Business tier. Implements business processes and other functional logic for the system.

Data tier. Provides persistent data storage.

Availability Sets. Create an Availability Set for each tier, and provision at least two VMs in each tier. This approach is required to reach the availability SLA for VMs.

Subnets. Create a separate subnet for each tier. Specify the address range and subnet mask using CIDR notation.

Load balancers. Use an Internet-facing load balancer to distribute incoming Internet traffic to the web tier, and an internal load balancer to distribute network traffic from the web tier to the business tier.

Jumpbox. A jumpbox, also called a bastion host, is a VM on the network that administrators use to connect to the other VMs. The jumpbox has an NSG that allows remote traffic only from whitelisted public IP addresses. The NSG should permit remote desktop (RDP) traffic if the jumpbox is a Windows VM, or secure shell (SSH) requests if the jumpbox is a Linux VM.

Monitoring. Monitoring software sush as Nagios, Zabbix, or Icinga can give you insight into response time, VM uptime, and the overall health of your system. Install the monitoring software on a VM that's placed in a separate management subnet.

NSGs. Use network security groups (NSGs) to restrict network traffic within the VNet. For example, in the 3-tier architecture shown here, the data tier does not accept traffic from the web front end, only from the business tier and the management subnet.

Key Vault. Use Azure Key Vault to manage encryption keys, for encrypting data at rest.

Recommendations
VNet / Subnets
When you create the VNet, allocate enough address space for the subnets you will need. Specify the address range and subnet mask using CIDR notation. Use an address space that falls within the standard private IP address blocks.

Don't pick an address range that overlaps with your on-premise network, in case you need to set up a gateway between the VNet and your on-premise network later. Once you create the VNet, you can't change the address range.

Design subnets with functionality and security requirements in mind. All VMs within the same tier or role should go into the same subnet, which can be a security boundary. Specify the address space for the subnet in CIDR notation. 

Load balancers
The external load balancer distributes Internet traffic to the web tier. Create a public IP address for this load balancer. Example:

azure network public-ip create --name pip1 --location westus --resource-group rg1
azure network lb create --name lb1 --location --location westus --resource-group rg1
azure network lb frontend-ip create --name lb1-frontend --lb-name lb1 --public-ip-name pip1 --resource-group rg1
For more information, see Get started creating an Internet facing load balancer using Azure CLI

The internal load balancer distributes network traffic from the web tier to the business tier. To give this load balancer a private IP address, create a frontend IP configuration and associate it with the subnet for the business tier.
