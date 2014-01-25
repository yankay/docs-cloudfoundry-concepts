---
title: Scaling Cloud Foundry
---

To increase the capacity and availability of the Cloud Foundry platform, you can scale a deployment up using the strategies described below.

## <a id='capacity'></a>Scaling Platform Capacity ##

You can scale platform capacity vertically by adding memory and disk, or horizontally by adding more VMs running instances of Cloud Foundry components.

<%= image_tag("scale_cf.png", :height => "450px", :width => "475px") %>

### <a id='tradeoffs'></a>Tradeoffs and benefits ###

The nature of a particular application should determine whether you scale vertically or horizontally.

**DEAs**:

The optimal sizing and CPU/memory balance depends on the performance characteristics of the apps that will run on the DEA.

 * The more DEAs are horizontally scaled, the higher the number of NATS messages the DEAs generate. There are no known limits to the number of DEA nodes in a platform.
 * The denser the DEAs (the more vertically scaled they are), the larger the NATS message volume per DEA, as each message includes details of each app instance running on the DEA.
 * Larger DEAs also make for larger points of failure: the system takes longer to rebalance 100 app instances than to rebalance 20 app instances.

**Router**:

Scale the router with the number of incoming requests. In general, this load is much less than the load on DEA nodes.

**Health Manager**:

The Health Manager works as a failover set, meaning that only one Health Manager is active at a time.
For this reason, you only need to scale the Health Manager to deal with instance failures, not increased deployment size.

**Cloud Controller**:

Scale the Cloud Controller with the number of requests to the API and with the number of apps in the system.

## <a id='availability'></a>Scaling Platform Availability ##

To scale the Cloud Foundry platform for high availability, the actions you take fall into three categories.

 * For components that support multiple instances, increase the number of instances to achieve redundancy.
 * For components that do not support multiple instances, choose a strategy for dealing with events that degrade availability.
 * For database services, plan for and configure backup and restore where possible.

**Note**: Data services may have single points of failure depending on their configuration.

### <a id='processes'></a>Scalable processes ###

You can think of components that support multiple instances as scalable processes.
If you are already scaling the number of instances of such components to increase platform capacity, you need to scale further to achieve the redundancy required for high availability.

There are seven scalable processes:

* DEA: More DEAs add application capacity.
* Cloud Controller: More Cloud Controllers help with API request volume.
* Health Manager: Scaling to two Health Managers is sufficient.
* Router: Additional routers help bring more available bandwidth to ingress and egress.
* Loggregator Server: Deploying additional Loggregator servers splits traffic across them.
* Loggregator Router: Deploying additional Loggregator routers allows you to direct traffic to them in a round robin manner.
* UAA: Scaling to two UAAs is sufficient.

### <a id='single-node'></a>Single-node processes ###

You can think of components that do not support multiple instances as single-node processes.
Since you cannot increase the number of instances of these components, you should choose a different strategy for dealing with events that degrade availability.

First, consider the components whose availability affects the platform as a whole.

**HAProxy**:

In production deployments, it is a good idea to use your own load-balancing infrastructure with an associated high-availability strategy and configuration.
HAProxy is typically used for Cloud Foundry deployments where high availability is not a requirement.

**NATS**:

Cloud Foundry continues to run any apps that are already running even when NATS is unavailable for short periods of time.
An appropriate strategy in a high availability Cloud Foundry deployment would be to leverage vSphere high availability features to ensure that vSphere keeps the NATS node running in the event of a hardware failure.

**NFS Server**:

For some deployments, an appropriate strategy would be to use vSphere high availability features to immediately recover the VM where the NFS Server runs.
In others, it would be preferable to run a scalable and redundant blobstore service. Contact Pivotal PSO if you need help.

**SAML Login Server**:

An appropriate strategy would be to leverage vSphere high availability features to quickly recover the VM where the SAML Login Server runs.
You also can ensure that you enable the BOSH resurrector to recover the VM as quickly as possible.

Secondly, there are components whose availability does not affect that of the platform as a whole. For these, recovery by normal IT procedures should be sufficient even in a high availability Cloud Foundry deployment.

**Syslog**:

If it is important to avoid loss of logs, consider using vSphere to recover the Syslog VM quickly.
Otherwise, an event that degrades availability of the Syslog VM causes a gap in logging, but otherwise Cloud Foundry continues to operate normally.

**Collector**:

This component is not in the critical path for any operation.

**Compilation**:

This component is active only during platform installation and upgrades.

### <a id='databases'></a>Databases ###

For database services, plan to leverage vSphere high availability features and to configure backup and restore where possible.
Contact Pivotal PSO if you require replicated databases and you need assistance.
