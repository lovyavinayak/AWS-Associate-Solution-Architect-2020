# Scalability & High Availability

* Scalability means that an application / system can handle greater loads by adapting.
* There are two kinds of scalability:
    - **Vertical Scalability**
    - **Horizontal Scalability** (= elasticity)
* Scalability is linked but different to High Availability

**Vertical Scalability**
- Vertically scalability means increasing the size of the instance.
- **Example**: If an Application runs on a t2.micro scaling the application vertically means 
    running it on a t2.large.
- Vertical scalability is very common for non distributed systems, such as a database.
- **RDS, ElastiCache** are services that can scale vertically.
- There’s usually a limit to how much you can vertically scale (hardware limit).

**Horizontal Scalability**
- Horizontal Scalability means increasing the number of instances / systems for your application. 
- Horizontal scaling implies distributed systems.
- This is very common for web applications or modern applications. 
- It’s easy to horizontally scale thanks the cloud offerings such as Amazon EC2.

