

# ZTO EXPRESS: Development and Deployment on Production Environment Based on KubeSphere

Shared by Yang Xiaofei, head of R&D of ZTO Express’s Cloud Platform, this article refers to the introduction the development and deployment of KubeSphere on production environment, as well as application scenarios of ZTO Express.

##  Five Difficulties Encountered by ZTO Express

By the third quarter of 2020, the market share of ZTO Express surged to 20.8%, making the company lead the industry. Increasing complexity in management together with the raging development of ZTO Express calls for changes.

Five prominent difficulties were faced by ZTO Express:

**Diversified versions were required for different environments.**
As the project upgraded, multiple versions were carried out. Continue to apply virtual machine in resource response could hardly meet the demand.

**Frequent upgrading called for quick environment initialization.**
Upgraded versions were proposed frequently. New versions were proposed every week. 

**Resource application and environment initialization were over-complex.**
ZTO Express still resorted to conventional approaches of resource application in 2019, when trouble tickets were required for environment initialization delivery. It was suffering for testers as they needed to apply for resources first and release them after testing.

**Low utilization rate of existing virtual resource.**
Staff turnovers and changes in positions would send abundant resources into zombies, especially on development and testing environment. 

**Inadequate horizontal extension capacity.**
Resources were scarce in important shopping days such as “6.18” and “double 11”. The company used to prepare resources in advance and take them back after the events, which proved to be outdated.

## The Application of Cloud on Production Environment

Three steps are included, namely, cloud-based, cloud-ready and cloud-native.



![](/Users/xieyicen/Desktop/Technical-blogs/ZTO Express/image for ZTO/1.jpeg)



The micro-service is based on Dubbo framework with completed adjustment, while it is implemented through virtual machine. Given many problems crop up when abundant Salt took place simultaneously, it is essential to adjust IaaS and containers in line with evaluation.

![](/Users/xieyicen/Desktop/Technical-blogs/ZTO Express/image for ZTO/2.jpeg)



## KubeSphere Development and Deployment

### Why KubeSphere

ZTO Express decides to apply KubeSphere as the construction scheme of its container management platform, ZKE. In addition, agreement has been made between ZTO Express and QingCloud: private cloud products provided by QingCloud are applied to develop the IaaS of ZTO Express, and KubeSphere is taken as an upper container PaaS platform for running micro-services.

### Construction Direction

In line with the reality, KubeSphere is taken as the base to carry out stateless service, manage Kubernetes and infrastructure resources through visualization. Stateful services are provided in IaaS, like the middleware.

![](/Users/xieyicen/Desktop/Technical-blogs/ZTO Express/image for ZTO/3.jpeg)



In term of micro-service, Istio is applied first, which turns to be complex and it will cost much in adjustment.

![](/Users/xieyicen/Desktop/Technical-blogs/ZTO Express/image for ZTO/4.jpeg)



### Large Cluster With Multi-tenants or Small Cluster with Single Tenant

Multiple small clusters are adopted and divided in accordance with business scenarios (such as middle desk business, and scanning business) or resource applications (such as big data, edge). The DevOps platform mentioned above is taken for CI/CD and KubeSphere is taken as the support of containers. Users can check logs, deployment, reconstruction and other information at terminals.

Based on multi-cluster design, each cluster on development, testing and production environment are deployed with a set of KubeSphere, while public components are drawn out, such as monitor and log.

Given KubeSphere 2.0 supports only LDAP connection, KubeSphere group manages to integrate OAuth connection that can be provided by KubeSphere 3.0 into version 2.0 through an independent branch. After development and adjustment, the customized parameters in OAuth authentication of ZTO Express are also successfully integrated through scanning QR codes.

## Secondary Development Based on KubeSphere

This part refers to the introduction of customized development based on the integration of business scenarios and KubeSphere between the summer of 2019 and October of 2020.

### Super-Resolution

Super-Resolution is applied. Once limit is set, requests can be quickly computed and integrated. On production environment, the super-resolution ratio for CPU is 10 and memory 1.5.

###      CPU Cluster Monitoring

Usage information is measured to show the monitoring data of GPU cluster.

### HPA Horizontal Scaling

KubeSphere resource allocation supports horizontal scaling. Horizontal scaling that is set independently, together with super-resolution, facilitates the measurement of the super-resolution ratio.

Even in terms of some core businesses, the application of HPA and interface setting of KubeSphere largely reduces the need of operation and maintenance. Demand in emergency scenarios can be quickly responded, such as the demand of instantly increasing replication when upstream consumption backlogs.

### Batch Restart

Abundant deployments might be restarted under extreme conditions, and an exclusive module is set for dealing with it: the one-click setting of KubeSphere ensures instant restart and quick response of clusters or deployments under one Namespace.

### Affinity of Container

Soft anti-affinity is designed, as some applications found their resource usage mutually exclusive.  Some adjustments have been made and some affinity settings have been added.

### Scheduling Strategy

In term of scheduling strategy, advanced setting page of KubeSphere is applied. Some page elements are added, and functions including designation of host, designation of host group, and exclusive host are configured in the form of a table row. 

### Gateway

Each Namespace in KubeSphere holds an independent gateway, which meet production requirement of ZTO Express. However，pan-gateway is required in development and testing, as quicker responses to server are expected. Hence, a pan-gateway and independent gateway are set, and pan-domain name are applied by all development and testing. They are configured and lie out in KubeSphere interface, thus to ensure direct access to services.

### Log Collection

Fluent-Bit has been applied for log collection, while it has always failed as businesses kept increasing, which might be contributed by mistakes made in resources upgrading or parameters. Therefore, Sidecar is applied to collect logs. Services based on Java all set an independent Sidecar, and push logs to centers like ElasticSearch through Logkit, a small agent. Under development and testing environment, Fluent-agent is also adopted to collect logs. For production scenarios that require complete logs, logs are required to be persistently stored at disks. All logs of containers are collected through four approaches: console log, Fluent-agent console log, /data Sldercar-logkit and /data NFS.

### Event Tracing

Event tracing was added to KubeSphere and could be sent to Ding Talk. Changes in businesses that were highly concerned under production environment could be sent to work group of Ding Talk through customized configuration.

## Future Planning

### Service Plate

In the KubeSphere console interface, micro-services are shown in the form of table rows. Graphics are applied to directly demonstrate their relationships and key indexes, like events, logs, and emergencies, thus to realize visualization. In such case, it is expected that all individuals, including operators, maintainers as well as developers, can understand the framework of the services provided, the middlewares and data bases relied on, and the running status, such as service failure or potential problems behind services.

### Whole Domain PODS

The whole domain PODS is known as thermodynamic diagram in KubeSphere. It is expected that status quo of all PODS, including changes in color and resources allocation can be seen from the perspective of the whole cluster.

### Edge Computing

Based on the combination of edge computing and container, KubeEdge has been applied. Scenarios for the application of edge computing include:
***Uploading scanned statistics of transferred expresses***
Statistics of scanned transferred expresses will be treated by the service deployed at each transfer center and then uploaded to statistic center. Services deployed at each transfer center are released by remote automated scripts. ZTO Express holds nearly 100 transfer centers and 5 participators are required for each release per day. The application of edge management can largely reduce the labor and maintenance cost, and flexible release strategies can be applied based on Operator recommended by Kubernetes Community.

***Automatic recognition of violate practice of operator***
For reducing the breakage of expresses, cameras are equipped at each transfer center and branch for monitoring operators. Statistics collected will be uploaded to local GPU boxes and treated before being uploaded to statistic center. Applications on GPU boxes are manual released with low efficiency, and connections between boxes and statistic center are prone to break. The edge scheme of KubeEdge can effectively address problems of release and node monitoring.

***The implementation of wisdom park project***

The wisdom park project is being carried out in ZTO Express, and container technologies will be applied to address pain points of many edge scenarios. 

## Some Problems Expected to Be Solved

**Management of abundant edge nodes**

**Stability and high availability of KubeEdge**

**Deployment and automatic operation and maintenance of edge nodes**







