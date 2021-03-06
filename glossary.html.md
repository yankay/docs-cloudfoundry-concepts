---
title: Cloud Foundry Glossary
---

| Term          | Definition   |
| ------------- | ------------ |
| API           | Application Programming Interface |
| BOSH          | A deployment orchestration solution. |
| CLI           | Command Line Interface |
| DEA           | Droplet Execution Agent. The DEA is the component in Cloud Foundry responsible for staging and hosting applications. |
| Domains       | A domain is a domain name like `acme.com` or `foo.net`. Domains can also be multi-level and contain sub-domains like the “store” in `store.acme.com`. Domain objects belong to an organization and are associated with zero or many spaces within the organization. Domain objects are not directly bound to apps. |
| Droplet       | An archive within Cloud Foundry that contains the application ready to run on a DEA. A droplet is the result of the application staging process. |
| Management    | You can manage spaces and organizations with the cf command line interface, the Cloud Controller API, and the Cloud Foundry Eclipse Plugin. |
| Organizations | An organization is the top-most meta object within the Cloud Foundry infrastructure. If an account has administrative privileges on a Cloud Foundry instance, it can manage its organizations. |
| Routes        | A route, based on a domain with an optional host as a prefix, may be associated with one or more applications. For example, `www` is the host and `acme.com` is the domain when using the route `www.acme.com`. It is also possible to have a route that represents `acme.com` without a host. Routes are children of domains and are directly bound to apps. |
| Spaces        | An organization can contain multiple spaces. The defaults for a standard Cloud Foundry install are development, test, and production. You can map a domain to multiple spaces, but you can map a route to only one space. |
| Staging       | The process in Cloud Foundry by which the raw bits of an application are transformed into a droplet that is ready to execute. |
| Warden        | The mechanism for containerization on DEAs that ensures applications running on Cloud Foundry have a fair share of computing resources and cannot access either the system code or other applications running on the DEA. |