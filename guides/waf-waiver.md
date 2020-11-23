# WAF waivers
> Azure Web Application Firewall (WAF) on Azure Application Gateway provides centralized protection of your web applications from common exploits and vulnerabilities. Web applications are increasingly targeted by malicious attacks that exploit commonly known vulnerabilities. SQL injection and cross-site scripting are among the most common attacks.

> WAF on Application Gateway is based on Core Rule Set (CRS) 3.1, 3.0, or 2.2.9 from the Open Web Application Security Project (OWASP). The WAF automatically updates to include protection against new vulnerabilities, with no additional configuration needed.

> All of the WAF features listed below exist inside of a WAF Policy. You can create multiple policies, and they can be associated with an Application Gateway, to individual listeners, or to path-based routing rules on an Application Gateway. This way, you can have separate policies for each site behind your Application Gateway if needed.

See [official documentation](https://docs.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview) for more information.

If a waiver is required for any policy then this must be approved by Security and implemented by CCoE.

## Waiver process
- CCoE provide details of policy breach
- investigate policy breach
- take corrective action if policy breach can be avoided
- if policy breach cannot be avoided, capture details in [FFC WAF Waiver log](https://defra.sharepoint.com/:w:/s/pwa/Future%20Farming%20and%20Countryside%20Programme/EWW5fpKNiRtMk4DvLGr86bgB3pcVQJlLOqsHtH1t0eiZ-Q?e=geChTo)
- updating both Waiver table and WAF log output
- add justification for waiver
- forward to Security for approval
- if approved, request CCoE update policies
