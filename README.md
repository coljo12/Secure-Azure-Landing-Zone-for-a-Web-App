# Secure-Azure-Landing-Zone-for-a-Web-App
This project builds a secure, scalable, and cost-aware Azure environment to host a simple web application.

## ⚠️ Security Notice
**IMPORTANT:** This repository contains documentation with screenshots that may expose sensitive information. Please review the [SECURITY.md](SECURITY.md) file for a comprehensive security assessment and recommendations.

**Known Issues:**
- Some screenshots contain Azure Subscription IDs and personal information
- Documentation references HTTP traffic, which should be replaced with HTTPS-only

##  Goals

- Create a secure Azure landing zone using best practices.
- Deploy a web app with managed identity and HTTPS.
- Implement monitoring, logging, and cost controls.
- Document architecture and role-based contributions.


## Phase 1: Set Up and Planning
- In this phase, I am setting up my Azure Account and subscription that I was provided. I was given $200 in credit for Azure to use however I'd like so I will be using it for this project, which I feel highlights many technological skills I have been learning in the Server and Cloud Administration cohort through the Microsoft Software and Systems Academy, as well as just other skills I have been learning on my own.

**⚠️ Security Warning:** The image below contains sensitive information including Azure Subscription ID and personal information. In production environments, always redact or blur sensitive data before sharing screenshots publicly.

![Subscription Setup Notification](images/SubSetupNotif.png)

## Phase 2: Building the Core Infrastructure
- First, we are going to create a resource group and name it "ProjectResourceGroup," and use the East US region to better
ensure a solid balance of proximity and network conditions, with the Central US or East US 2 regions as fallback options depending on the service needs.
![Resource Group Creation](images/ResourceGroupCreation.png)

- The Second part of the infrastructure I am going to build is the Network Security Groups (NSG) and then the Virtual Network. I am building two NSGs, one called "ProjectNSG-Web"
and another called "ProjectNSG-Secure." These will both have different inbound and outbound rules to create a secure network for our Web App.
![Resource Group Overview](images/ResourceGroupOverview.png)

- The first NSG is the "ProjectNSG-Web," and I created an inbound rule to allow all HTTPS and HTTP traffic. I also created another inbound rule for my subnet that will be attached to the "ProjectNSG-Secure" to return the traffic from my Subnet called "SubnetWeb," which is attached to "ProjectNSG-Web." After that, I created a new "Deny-All-InBound" rule with a priority of 4000 because it was a custom rule, and I learned that if it's custom, the priority level must be from 100-4096. After that, I received an error message from that Deny All rule I just created because my default Load Balancer rule wouldn't work due to being a lower priority than my deny all rule, so I created a new rule for that as well and placed the priority just above the HTTPS and HTTP rule at 130.

**⚠️ Security Issue:** Allowing HTTP (port 80) traffic is a security risk. HTTP transmits data in plaintext and is vulnerable to man-in-the-middle attacks. **Recommendation:** Remove HTTP support and use HTTPS-only (port 443) with HTTP to HTTPS redirects at the application layer if needed. See [SECURITY.md](SECURITY.md) for details.

![NSG Web Rules Inbound](images/NSGWebRulesInbound.png)

- Next is to configure and create my outbound rules for all traffic for my "ProjectNSG-Web" network security group. The first outbound rule I created was called "Allow-Web-to-Secure," and this allows the backend access communication between my subnets. After that, I then created the next rule "Allow-HTTPS-HTTP-Outbound," allowing the outbound traffic to reach the internet via HTTPS and HTTP only, and the "Deny-All-Outbound," which is the final outbound rule I created, complements the previous rule because it blocks all other outbound traffic unless it's HTTPS or HTTP due to that rule's higher priority. I did this to really try and enforce zero trust.

**⚠️ Security Issue:** The outbound rule allows HTTP (port 80) traffic. For enhanced security, consider restricting to HTTPS-only (port 443). See [SECURITY.md](SECURITY.md) for comprehensive security recommendations.

![NSG Web Rules Outbound](images/NSGWebRulesOutbound.png)

---

## Security Best Practices

For a comprehensive security review and recommendations, please see [SECURITY.md](SECURITY.md).

### Key Security Considerations:
- **HTTPS-Only:** Disable HTTP (port 80) and use HTTPS (port 443) exclusively
- **Data Protection:** Redact sensitive information (subscription IDs, personal data) from public documentation
- **Zero Trust:** Implement defense in depth with multiple security layers
- **Monitoring:** Enable Azure Security Center and logging for all resources
- **Identity:** Use Azure Managed Identity and Azure Key Vault for secrets management
- **Network Segmentation:** Properly segment workloads across subnets with appropriate NSG rules

### Additional Resources:
- [Azure Security Best Practices](https://docs.microsoft.com/azure/security/fundamentals/best-practices-and-patterns)
- [Azure Network Security](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices)
- [Azure Landing Zone Documentation](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/)






