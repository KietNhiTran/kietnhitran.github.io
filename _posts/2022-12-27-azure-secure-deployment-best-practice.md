---
layout: post
title: Azure Secured Deployment Best Practices
date: 2022-12-27 20:44 +0700
categories: [Azure, Well-Architected]
tags: ["Security"]
---
Security is no negotiation. There is no exception to ignore or scarify security in any solution deployed in any cloud. Here is a simple 7 step best practice to implement a secured deployment on Azure. I believe the same principles can apply to any other cloud provider with equivalent.
![Secured Deployment 7 steps best practice](/assets/img/posts/Azure-secured-deployment-best-practices.jpg)
## 1. Protect your data where the data is stored at storage level.
- Putting restriction logic at database like row based access control, RABC control with Azure Active Directory integration,.
- Encrypted at rest
- Data classification & data masking
Most of Azure databases and data storage have built-in world class security features mentioned above like Azure SQL database, Synapse. Choose the right database and storage which fit minimum security requirements with less effort.

## 2. Keep data in side Azure private network
By using "private link" or "service endpoint" when accessing Azure PaaS service and blocking public access, you reduce attack surface to your solution.

## 3. Using cloud native secret management solution
keeping all secret keys and credentials in Key Vault. Accessing key vault via Managed Identity and RBAC in runtime. This will avoid storing secrets and credentials in source code, script, and notebook. Key vault has built-in high availability and disaster recovery which prevents your secret lost.

## 4. Securing your network
Applying security to your network and infrastructure via a zero trust layer approach, stopping attach at edge, using network isolation and controlling route and setting up baston host to allow access from on-premise to cloud infrastructure.
- Froot door with Web application firewall can cache and absorb big traffic to your solution and protect it with pre-built web related security rule.
- DDoS standard plan will protect your solution from Service Deny attack.
- Use network isolation to separate each tier of your solution.
- Use firewall to prevent data exfiltration, routing traffic and inspect and log traffic for monitoring.
- Use regional Application Gateway with Web Application Firewall to achieve regional web traffic protection.
- Finally, using network security group, or application security group in conjunction with user define route to control traffic.

## 5. All access must be authenticated and authorized via ADD and RBAC. 
Azure active directory provides fine grain identity access and management. 
- Apply least privilege principle when assigning role to user.
- Turn on Multi-Factor Authentication
- Use Privilege Identity Management for high privilege users. 
- Whenever it is possible, using managed identity to access service and using service principle for automation.

## 6. Monitoring and auditing
- Turn on the activity log for auditing.
- Collect platform log and application log to Azure Log Analytics for central management and distributed tracing. 
- Monitor solution via Azure Monitor. 
- Setting up notification and looking after your environment via Azure Defender for cloud.

## 7. Governance and repeating organizational standards at scale.
- Use Azure policy to control compliance. 
- Use Azure blueprints / IaC template to have consistent standard setup across your environment.
