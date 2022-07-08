###### tags: `GCP`

Core Infrastructure Overview
===
# Resource and Access
## Resource Hierarchy
- 主要可將所有資源分成Organization, Folder, Project, Resources (4 層)
- Add policy於Folder，則下面所有資源都須符合該policy，因此要注意對於policy的訂定關係 (Policy are inherited downward)

<center> 

![](https://i.imgur.com/3CUKGDe.png =350x)

</center>

### Resourse (Lv1)
- VM, cloud storage bucket, data sheet, bigquery, etc 
### Project (Lv2)
- Managing API, Enabling billing, adding/removing collaborators, other google services.
- Project 由Project ID, Project name, Porject number 三者來定義
![](https://i.imgur.com/7VcpJiv.png)
- **Resource Manager Tool**: 一個API可以幫助管理Project。
<center>
    
![](https://i.imgur.com/874eaF6.png =250x)

</center>

### Folder (Lv3)
- ```Folder```底下可以有```Project```也可以有其他```Folder```
- 可以將不同部門視作一個```Folder```來去進行管理 e.g. Dep1, Dep2。在該```Folder```下，不同```Project```服從相同Policy，在進行Project新增及複製時不需要再去進行```Policy```設定
### Organization (Lv4)
- 可以設置Organization policy administrator & Project creator讓相關人員進行管理。
- 若在自己帳號的workspace中，```Project```會直接屬於你自己的Organization。若不是自己的workspace，則需要利用```IAM```去產生相對應身分

## IAM
- 由左到右，越多限制。
![](https://i.imgur.com/WbPE6hV.png =500x)
- Basic role
    - Owner 
    - Editor
    - Viewer
    - Billing Admin
- Predefined role (When several people work together with sensitive data) (provide specific service in a project)
- Custom role
    - need to manage the permissions for the custom role
    - Custom roles can only be applied to either ```Project level``` or ```Organization level```



## Interact with Google Cloud 可以使用哪些媒介使用GCP
1. Google Console
2. Cloud SDK & Cloud Shell 
    - ```gcloud```: provide the main command line interface
    - ```gsutil```: provide access to Cloud Storage from the command line
    - ```bq```: BigQuery command line
    - Cloud Shell: a Debian-based virtual machine with a persistent 5GB home directory
3. API
    - use ```Google APIs Explorer``` to search
    - control your code version/ service 
4. Cloud Console Mobile App
    - Start, stop, and use SSH to connect into ```Compute engine```, see logs
    - Start stop ```Cloud SQL isntances```
    - Demploy on ```App Engine```
    - billing information
    - view error, alert
    - download ```cloud.google.com/console-app```

![](https://i.imgur.com/7IkjC41.png)
Check 這題答案


# VM and Networks
## Virtual Private Cloud (VPC)
由Public Cloud host但東西皆為private，表示可以用Cloud resource，但仍保有其隱密性
<center>
    
![](https://i.imgur.com/oEMn2x6.png =500x)

</center>

- using firewall rules to restrict access to instances
    - no router provisioning or managing
    - restrict access to instance
    - rules can be defined through metadata tags
- segmenting networks
- global that can have subnets in any ```Cloud region``` (can be used to build solutions that are resilient to disruptions) (networks are global, but subnets are regional)
- no external IP address required

<center>
    
![](https://i.imgur.com/fRoTOXj.png =450x)

</center>

當同一個region裡面有兩個compute engine其中一個出問題，另一個還能繼續正常運做

### VPC peering & Shared VPC
- VPC 實際上還是屬於Google Cloud Projects，若組織內有多個Google Cloud Projects及VPCs可以透過**VPC peering**及**Shared VPC**的方式進行溝通

### Cloud Load Balancing
- fully-distributed managed service
- put in front of all your traffic: **HTTPs, TCP traffic, SSL traffic, UDP traffic**
### Cloud DNS & Cloud CDN
![](https://i.imgur.com/CYmkwTy.png)
### The way to connect Google VPC

- IPsec VPN to create a tunnel connection through internet
    - ```cloud router``` to make the connectoin dynamic
    - Let other networks and Google VPC exchange route information over the VPN using ```Border Gateway Protocol```
    - Security issue, and bandwidth reliability because of internet
- Direct Peering
    - When concerning about security issue or bandwidth reliability
    - put a router in the same public datacenter as a Google point of presense (PoP)
    - use a router to exchange traffic between networks
- Carrier Peering 
    - When customers that are not ready to the PoP
    - Work with a partner by giving direct access from an on-premises network through a service provider's network
- Dedicated Inerconnect
    - When need higher uptime connection 
    - Allow for one or more direct, private connections to Google
    - Can be covered by up to a 99.99% SLA
    - Connection can be backed up by a VPN
- Partner Interconnect
    - Provide connectivity between an on-premises network
> 1. Netork Uptime: the time when a network is up and running
> 2. Downtime: the time when a network is unavailable
> 3. Service level agreements (SLAs): promise a set of performance standards between a service provider and their client
> 4. On-premise 公司內部環境運行的軟體 (A company hosts everything in-house in an on-premise environment, while in a cloud environment, a third-party provider hosts all that for you. )

![](https://i.imgur.com/ijFlpuS.png)

![](https://i.imgur.com/Jd7mYxM.png)

> DNS (domain name system): web protocal
> CDN: related to edge computing 減少latency加速local spped (also save money)
> DNS+CDN connect to web servers running HTTP web service (compute engine)


Question: An application running in a Compute Engine virtual machine needs **high-performance scratch space**. Which type of storage meets this need?  Ans: **Local SSD**

## Compute Engine

# Storage
```Cloud Storage```
```Cloud Bigtable```
```Cloud SQL```
```Cloud Spanner```
```Filestore```
## Four Cloud Storage Classes
- Standard
- Nearline
- Coldline
- Archive

# Containers

## Kubernetes ([link](https://hackmd.io/LLmGLX1DSNmuBThLmkCw4Q))


## Google Kubernetes Engine (GKE)
```gcloud command``` or ```google cloud console``` 
- Deploy and manage applications
- Perform administration tasks
- Set policy
- Monitor workload health
- Automatic scaling of your cluster's node instance count...

```shell=
$ gcloud container clusters create <name>
```

## Hybrid and multi-cloud
- Spread the computing workload over two or more networked servers
- Containers break these workloads down into microservices

[Anthos]()
- a hybrid and multi-cloud solution
- fraemwork rests on K8S and GKE On-premise
- provide a set of tools for monitoring and maintenance


# Applications in the Cloud
## ```App Engine``` 
![](https://i.imgur.com/Qaf0m9V.png =600x)
Coding options: ```Eclipse, IntelliJ, Marven, Git, Jenkins, PyCharm```
- No servers to provision or maintain
- Provides built-in services & APIs: ```NoSQL datastores, Memcache, Load balancing, Health checks, Application logging, user authentication API```
- Also provide SDK to help develop, deploy, and manage your apps
- manage the hardware and networking infrastructure required to run your code
- suitable for developing and hosting a web application 
### Standard environment vs. flexible environment
Standard environment
- use Containers (specified version)
- Run in a secure sandbox environment
    - can distribute request across multiple servers and scale servers to meet traffic demand
    - 等同獨立於hardware, OS, physical location的安全性
1. Persistent storage with queries, sorting, and transactions
2. Automatic scaling & load balancing
3. Scheduled tasks for triggering events at specified times or regular intervals
4. Integration with other Google Cloud services and API
- Three steps for standard environment
    1. **Develop** web app and test locally
    2. **Deploy** to App Engine with SDK
    3. App Engine **scales** and services the app
Flexible environment
![](https://i.imgur.com/fxHC9LZ.png)

Standard environment
K8S
Flexible environment


## ```Cloud Endpoints``` 
- **Distributed API management system**
- maintain API with low latency and high performance
- provide an API console, hosting, logging, monitoring
- support running in ```App Engine, GKE, Compute Engine```

## ```Apigee Edge```
- specific focus on **business problem**, like rate limiting, quotas, and analytics
- Backend services for Apigee Edge don't need to be in Google Cloud

## ```Cloud Run```: 拿來部署程式套件到雲端，不需要處理infrastruture, maintain, security等等狀況。只需要focus在開發，用Container方式部署
- a managed compute platform that can run stateless containers
- **Serverless**, removing the need for infrastructure management
- built an Knative, open API, environment built on K8S
- can only pull images from **Artifact registry**
> Artifact registry: A single place for your organization to manage container images and language packages

![](https://i.imgur.com/2FLjkY7.png =600x)
![](https://i.imgur.com/014uT2R.png =600x)
啟用Compute Engine後就會一直收費，上述Container除了在啟動、結束、Handle request時會進行收費。可以減少大量減少idle時間的花費。

# Develop and deploy in the cloud
主要三個工具
1. Cloud Source Repositories
2. Cloud Function
3. Terraform

> Question: 通常開發使用Git repository，那有沒有辦法使用Google Cloud服務，但仍保持開發程式的隱私性? 
> Ans: 使用**Cloud Source Repositories**，透過**IAM permission的設定**即可達到且需要自己去maintain Git instance


![](https://i.imgur.com/IjpfVjT.png)
With **Cloud source repositories**, we can have any number of **private git repositories**.
![](https://i.imgur.com/KMAK5XJ.png)
可以將Github或BitBucket內的repository migrate到Cloud Source Repository，使用```Debugger & Error Reporting```等服務在不影響程式運行的情況下，trace所有在production上的錯誤。

## ```Cloud Functions``` 
- Lightweight, event-based, asynchronous compute solution
- allows you to create small, single-purpose functions that respond to cloud events without the need to manage a server 
    - 應用: 影像資料處理，resize, augmentation等等需要Compute resource介入並將transform後的資料轉換成需要的file
- only billed to the nearest 100 milliseconds and only while your code is running

## ```Terraform```
- 用來設計Template，當需要常常部署類似的環境時，要一一設定會非常耗時。因此，可以透過Terraform來設計Template加速部署
- 可以根據Template更新相關設定或加入新的需求
- Create a template file using **HashiCorp Configuration Langauge (HCL)** that describes what the compoentns of the environment should look like
- use the template to determine the actions needed to **create the environment your template describes**
- use ```Terraform``` to update the environment to match the change
- store and version control ```Terraform template``` in **Cloud Source Repositories**
- **Can be used as an infrastructure management system for Google Cloud resources**

# Logging and Monitoring
- **Monitoring $\to$ Logging $\to$ Error reporting $\to$ Debugging**
- Site Reliability Engineering (SRE): Collecting, processing, aggrgating and displaying real-time quantitative data about a system
- Monitoring: 顯示最緊急需要處理的error, Helps improve an application experience
![](https://i.imgur.com/SStmEkV.png)
- SRE 主要focus在與Product最相關的問題監控
![](https://i.imgur.com/MJVuHUi.png =600x)
- Testing: preferably automated testing in a refined CICD release pipeline
- Continual improvement $\to$ DashBoard $\to$ Automated alerts $\to$ Monitoring tools (提供最重要的問題資訊)

## System Performance & Reliability 
- Latency
- Traffic: 衡量有多少request會進到系統內，同時可以做為Capacity Planning
- Saturation: 負載量佔比(了解哪些資源最缺乏)
- Errors


## SLIs, SLOs, SLAs
- Service level indicator (SLIs)
    - Carefully selected monitoring metrics that measure one aspect of a service's reliability
    - metrics: (Number of good events) / (Count of all valid events)
- Service level Objective (SLOs)
    - combine a SLI with a target reliability (通常可靠度會趨近於100% e.g. 99.99%)
    - 通常需要滿足SMART原則 (Specific, Measurable, Achievable, Relevant, Time-bound)
- Service level Agreement (SLA) 對使用者系統使用上的承諾
    - **Commitments** made to your customers that your systems and applications will have **only a certain amount of downtime**
<center>
    
![](https://i.imgur.com/mzp9Wl8.png =400x)
    
</center>

**Monitoring $\to$ Logging $\to$ Error reporting $\to$ Debugging**
Use Google ```Integrated observability tools```
![](https://i.imgur.com/wvO6L5e.png)

## Monitoring Tools (```Cloud Monitoring```)
DevOps Teams first would like to monitor the systems
- BigQuery
    - how many queries in flight, how many data bytes scanned to the bills, data slots usage pattern
- Cloud run
    - Running containeriezed application to know cpu and memory utilization
- Custom Application
    - also can use open source opentelemetry to create their own metrics
- Compute Engine
- e.g. monitor cassandra, nginx, Apache Web Server, etc.

## Logging Tools (```Cloud Logging```)
- Define metrics based on your logs
- Log analysis, Log explorer
- Can be exported **as files to Cloud Storage**, **as messages through Pub/ Sub**, **into BigQuery Tables** (可儲存備份)
- Log retention
    - Retained by default for 30 days and up to a maximum 3650 days
    - Admin logs are stored by defualt for 400 days
![](https://i.imgur.com/jgoqa7E.png)
    
## Error reporting and debugging tools (```Error Reporting & Debugger```)
```Error Reporting```
- Count, analyze, and aggregates the crashes in your running cloud service
- **Error details:** time chart, occurrences, affected user count, first- and last- seen dates
- Create alerts to receive notificatoins on new errors
```Debugger```
- while running in production, examine code's function and performance under actual production conditions
- Easy **collaboration** with other team by **sharing debug sessions with a Console URL**
- Can integrate with version control system, e.g. Cloud Source Repo, GitHub
```Cloud Trace```
- Collect latency data from distribute application and display in the Google Cloud Console
- Trace from applications deployed on App Engine, Compute Engine, K8S
```Cloud Profiler```
- Use statistical techniques and extremely low-impact instrumentation running across all production application instances
- analyze application running anywhere, even other cloud platforms