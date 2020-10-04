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
    - From: t2.nano - 0.5G of RAM, 1 vCPU
    - To: u-12tb1.metal – 12.3 TB of RAM, 448 vCPUs          
- Vertical scalability is very common for non distributed systems, such as a database.
- **RDS, ElastiCache** are services that can scale vertically.
- There’s usually a limit to how much you can vertically scale (hardware limit).

## Horizontal Scalability
- Horizontal Scalability means increasing the number of instances / systems for your application. 
- Horizontal scaling implies distributed systems.
- This is very common for web applications or modern applications. 
- It’s easy to horizontally scale thanks the cloud offerings such as Amazon EC2.
- Scaling in or scaling out.
- Can be achieved via Auto Scaling Group or Load Balancer.

## High Availability
- Run instances for the same application across multiple AZs. 
- Can be achieved via ASG in multiple AZs or Load Balancer in multiple AZs.

