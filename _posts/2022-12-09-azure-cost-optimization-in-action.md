---
layout: post
title: Azure Cost Optimization in Action
date: 2022-12-09 21:04 +0700
categories: [Azure, Well-Architected]
tags: ["Cost optimization"]
---
The reason to run your solution on the cloud is agility and scalability to launch any resource you need and when you need it. It is not always a low-cost option. Therefore, to minimize your spending, cost optimization should be a continuous action. In fact, based on [Flexera Releases 2021 State of the Cloud Report Press Release](https://www.flexera.com/about-us/press-center/flexera-releases-2021-state-of-the-cloud-report), optimize cloud costs is the top initiative for the fifth year in a row. Let’s explore an actionable checklist to reduce your cloud cost.

To be able optimize cost, you need to be able to **observer your expense**, find opportunity to **optimize**, and **control it**. Let’s look at those cost optimization principles in action. 

## Observation in action
- **Tagging** your resource to identify them in your cost report. 
- Use **azure cost analysis** understand your cost spend and forecast. Combining with tagging, you can answer questions like what my cost is, where it is spent, in which project, environment or which team.
- Create **alert** or create **recommendation digest** to direct azure cost saving recommendations directly to your inbox.
- If you are an Enterprise Agreement contract customer, install [**Azure Cost Management PowerBI App**](https://appsource.microsoft.com/en-us/product/power-bi/costmanagement.azurecostmanagementapp) to have comprehensive cost review, analysis, saving recommendation and forecast. 
    
## Optimization in action

### Right configuration
- Act for each advised cost optimization recommendations under the Azure advisor. The full list of cost optimization recommendations include **right sizing**, **underutilization resources**, etc..
- Use cost optimal configuration in dev/test environment, such as:
    - Smaller size of VM
    - User B-series 
    - Turn off zone redundance setting in all your workload (for example: AKS, VM, …)
    - Use one node pool or shared AKS cluster for all dev/test services.
    - Relax high cost security features like: WAF on App gateway; DDoS; Firewall, but still keep everything unreachable via internet.
- Either terminate test environment when not using or stop all idle resources.
- If you are using App Service, consider using Premium V3 over Pv2 with comparable configuration with 20% saving (check the App service pricing page for detail).
- Using [Dev/Test subscription] (https://aka.ms/devtest) (if applicable) to save up to 60% cost.

### Reduce Waste
- Remove all unattached resources like unattached managed disk, public IPs, network interface. 
- Remove all empty backend targets for all routing services like load balancer, application gateway. Without a backend pool, these resources are useless but still cost you money. 
- Identify idle resources and review whether they should be stopped, deallocated, or deleted to save cost. Among those resources, VM, AKS cluster, VPN gateway, App Service are costly. Acting soon will save you money.
- Identity idle network components like Azure Firewall, VPN gateways and remove them. 
Tips: You can easily identify idle resource using [Azure Resource Graph query](https://learn.microsoft.com/en-us/azure/governance/resource-graph/first-query-portal). 
    
### Data Storage cost optimization
- Using B-Series for workload suitable for burst performance. That is workload with consistent or idle for long time but burst occasionally.
- There are four different tiers in Azure storage account. You might be surprised how much you can save when you organize data into tiers when workload has a lot of blob data. So, take action like:
    - Check unmodified blob for a long time then move it to lower cost tier. You can do this via setting up blob inventory report for observation and then take action
    - If you know your data access frequency, use data lifecycle management to move blob data between tiers.
    - Azure offers the ability to boost disk storage IOPS and MB/s performance. Consider using disk bursting for some scenarios such as improve startup times, handle batch jobs, traffic spikes https://docs.microsoft.com/azure/virtual-machines/disk-bursting
    
### Network cost optimization
- Check for cross region data transfer cost and consider if that is something you should act on.
- Look for high cost resources like DDoS, Firewall. Consider a lower cost alternative if that is applicable. 

### Reservation & Saving Plan
- There are [27 different services on Azure offer reservation]({{site.url}}/assets/img/costop/reservation-list.png). By using reservation, you can save up to 50% cost. Checkout by going to Azure, then reservations. Choose the service you are interested in, recommendations will show up based on your usage data. Check back frequently since your usage will change over time.
- Using [**Azure Calculator**](https://azure.microsoft.com/en-us/pricing/calculator/) to understand cost before choosing what to purchase and which option.
- Checkout Azure Saving plan recommendation under Azure advisor or select Saving Plan purchase. Act according to the recommendation to save up to 65% of cost for application resources.

### Design: Serverless and PaaS services adoption
- Using in-house technology, and virtual machines on cloud do require operation, maintaining effort and human labor cost. This is also a cost to be optimized. Review your workload and look for opportunity to adopt **cloud native technology** or **Azure PaaS service**, like AKS, Azure managed database, Azure managed cache, etc.
- You can even optimize operation effort further by leveraging **serverless** technology like Azure Function, Azure Container Apps, App Logic.

## Control in action
- In Azure cost management page, set your **azure budget** and **alert** to control cost expense.
- Using Azure cost optimization **policy** to governance at scale.  For example, specify allowed resource SKU to be prevision in development phrase via [Allowed virtual machine size SKUs policy](https://learn.microsoft.com/en-us/azure/governance/policy/samples/built-in-policies#compute). Check out Azure cost related Policy or build your own to governance as scale.

These are foundation principles that you can run through to get first actions in your cost optimization journey. For each type of resource, it has its own cost optimization guidance. For example: [Optimize compute costs on Azure Kubernetes Service (AKS)] (https://learn.microsoft.com/en-us/training/modules/aks-optimize-compute-costs/); [Azure Database for PostgreSQL and cost optimization](https://learn.microsoft.com/en-us/azure/architecture/framework/services/data/azure-db-postgresql/cost-optimization). Therefore, besides the general principles, frequently check on in-deep technique of each resource type used in your deployment architecture.

Saving costs is important, but it is not a unique factor affecting your decision. Your design should take in [tradeoffs between cost optimization, security, reliability, performance and operability](https://learn.microsoft.com/en-us/azure/architecture/framework/cost/tradeoffs). The attached link is a short documentation which would explain this concept perfectly clearly.

