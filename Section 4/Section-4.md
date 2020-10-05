# Scalability & High Availability

* Scalability means that an application / system can handle greater loads by adapting.
* There are two kinds of scalability:
    - **Vertical Scalability**
    - **Horizontal Scalability** (= elasticity)
* Scalability is linked but different to High Availability

## Vertical Scalability
- Vertically scalability means increasing the size of the instance.
- Involves scaling up or scaling down
- **Example**: If an Application runs on a t2.micro scaling the application vertically means 
    running it on a t2.large. 
- Vertical scalability is very common for non distributed systems, such as a database.
- **RDS, ElastiCache** are services that can scale vertically by upgrading the underlying instance type.
- There’s usually a limit to how much you can vertically scale (hardware limit).

## Horizontal Scalability
- Horizontal Scalability means increasing the number of instances / systems for your application. 
- Horizontal scaling implies distributed systems.
- This is very common for web applications or modern applications. 
- It’s easy to horizontally scale thanks the cloud offerings such as Amazon EC2.
- Scaling in or scaling out.

## High Availability
- High Availability usually goes hand in hand with horizontal scaling
- High availability means running your application / system in at least 2 data centers (== Availability Zones)
- The goal of high availability is to survive a data center loss
- The high availability can be passive (for RDS Multi AZ for example)
- The high availability can be active (for horizontal scaling)

## High Availability and Scalability for EC2

- **Vertical Scaling**: Increase instance size (scale up or down)
    - From: t2.nano - 0.5G of RAM, 1 vCPU
    - To: u-12tb1.metal – 12.3 TB of RAM, 448 vCPUs
- **Horizontal Scaling**: Increase number of instances (scale out or scale in)
    - **Scale in**: Decrease number of instances
    - **Scale out**: Increase number of instances
- Can be achieved by using ASG or Load Balancer
- **High Availability**: Run instances for the same application across multiple AZs.
    - Can be achieved via ASG multiple AZs or Load Balancer multiple AZs.

# Load Balancing
- Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream.

**Why use a Load Balancer?**
- Spread load across multiple downstream instances
- Expose a single point of access (DNS) to your application. EC2 instance information is not needed. Only need to know the hostname of the load balancer.
- Seamlessly handle failures of downstream instances
- Do regular health checks to your instances
- Provide SSL termination (HTTPS) for your websites
- Enforce stickiness with cookies
- High availability across zones. Load balancer and EC2 instance can span across multiple AZs. 
- Separate public traffic from private traffic
- **Public Traffic**: Traffic from users to load balancers
- **Private Traffic**: Traffic from load balancer to EC2 instances

**Why use an EC2 Load Balancer (ELB)?**
- An ELB is a managed load balancer
    - AWS gurantees that it will be working
    - AWS takes care of upgrades, maintenance, high availability 
    - AWS provides only few configuration knobs
- It costs less to setup your own LB but it be a lot more effort on your end
- It is integrated with many AWS services

**Health Checks**
- Health Checks are crucial for Load Balancers
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
- The health check is done on a port and a route (/health is common)
- If the response is not 200 (OK), then the instance is unhealthy
- Happens every n seconds and the time interval can be modified

## Types of Load Balancers [Need to know for the exam]
- AWS has 3 kinds of managed Load Balancers
    - **Classic Load Balancer (v1 - old generation) – 2009**: HTTP, HTTPS, TCP
    - **Application Load Balancer (v2 - new generation) – 2016**: HTTP, HTTPS, WebSocket. **Should be on the exam**
    - **Network Load Balancer (v2 - new generation) – 2017**: TCP, TLS (secure TCP) & UDP
- Overall, it is recommended to use the newer / v2 generation load balancers as they provide more features
- You can setup internal (private) or external (public) ELBs
- Review slide 84

**Classic Load Balancer**
- Supports TCP (Layer 4), HTTP & HTTPS (Layer 7)
- Health checks are TCP or HTTP based
- Fixed hostname XXX.region.elb.amazonaws.com

**Application Load Balancer (v2)**
- Application load balancers is Layer 7 (HTTP)
- Load balancing to multiple HTTP applications across machines (target groups)
- Load balancing to multiple applications on the same machine (ex: containers)
- Support for HTTP/2 and WebSocket
- Support redirects (from HTTP to HTTPS for example)
- Routing tables to different target groups
    - Routing based on path in URL (example.com/users & example.com/posts)
    - Routing based on hostname in URL (one.example.com & other.example.com)
    - Routing based on Query String, Headers (example.com/users?id=123order=false)
- ALB are a great fit for micro services & container-based application (example: Docker & Amazon ECS)
- Has a port mapping feature to redirect to a dynamic port in ECS
- In comparison, we’d need multiple Classic Load Balancer per application
- Check slide 89

**Application Load Balancer (v2) Target Groups**
- EC2 instances (can be managed by an Auto Scaling Group) – HTTP
- ECS tasks (managed by ECS itself) – HTTP
- Lambda functions – HTTP request is translated into a JSON event
- IP Addresses – must be private IPs
- ALB can route to multiple target groups
- Health checks are at the target group level
- Fixed hostname (XXX.region.elb.amazonaws.com)
- The application servers don’t see the IP of the client directly
    - The true IP of the client is inserted in the header X-Forwarded-For
    - We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)

**Network Load Balancer (v2)**
- Network load balancers (Layer 4) allow to:
    - Forward TCP & UDP traffic to your instances
    - Handle millions of request per seconds
    - Less latency ~100 ms (vs 400 ms for ALB)
**NLB has one static IP per AZ, and supports assigning Elastic IP** (helpful for whitelisting specific IP)
- NLB are used for extreme performance, TCP or UDP traffic
- Not included in the AWS free tier    

**Load Balancer Stickiness**
- It is possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer
- This works for Classic Load Balancers & Application Load Balancers
- The “cookie” used for stickiness has an expiration date you control
- Use case: make sure the user doesn’t lose his session data
- Enabling stickiness may bring imbalance to
the load over the backend EC2 instances




