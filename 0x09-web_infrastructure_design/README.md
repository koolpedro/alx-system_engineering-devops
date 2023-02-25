0x09. Web infrastructure design

General
You must be able to draw a diagram covering the web stack you built with the sysadmin/devops track projects

You must be able to explain what each component is doing

You must be able to explain system redundancy

Know all the mentioned acronyms: LAMP, SPOF, QPS

Requirements

General
A README.md file, at the root of the folder of the project, is mandatory

For each task, once you are done whiteboarding (on a whiteboard, piece of paper or software or your choice), take a picture/screenshot of your diagram

This project will be manually reviewed:
As each task is completed, the name of that task will turn green

Upload a screenshot, showing that you completed the required levels, to any image hosting service (I personally use imgur but feel free to use anything you want).

For the following tasks, insert the link from of your screenshot into the answer file

After pushing your answer file to GitHub, insert the GitHub file link into the URL box

You will also have to whiteboard each task in front of a mentor, staff or student - no computer or notes will be allowed during the whiteboarding session

Focus on what you are being asked:

Cover what the requirements mention, we will explore details in a later project

Keep in mind that you will have 30 minutes to perform the exercise, you will get points for what is asked in requirements

Similarly in a job interview, you should answer what the interviewer asked for, be careful about being too verbose - always ask the interviewer if going into details is necessary - speaking too much can play against you

In this project, again, avoid going in details if not asked



TASKS

0. Simple web stack
What is a server?
A server is a computer program or a physical computer system that provides services or resources to clients or other computers over a network. It can provide various services such as file sharing, email, web hosting, database management etc

What is the role of the domain name?
To provide a unique, easy-to-remember address that can be used to locate and access websites, email servers, and other resources on the internet.

What type of DNS record www is in www.foobar.com?
is a subdomain, and it is typically used to indicate a website on the internet.

What is the role of the web server?
The role of the web server is to receive and respond to HTTP requests from clients (such as web browsers) and deliver web pages and other content to them.

What is the role of the application server?
The role of the application server is to provide software services and functions that are required by web applications. This can include managing user authentication and access control, processing user inputs, managing sessions and state information, and more. 

What is the role of the database?
The role of the database is to store and manage the data that is required by web applications.

What is the server using to communicate with the computer of the user requesting the website?
TCP/IP Protocol, which allows data to be transmitted between the server and user computersecurely over the internet.

The infrastructure described above suffers from several issues:
1.	SPOF (Single Point of Failure): The infrastructure appears to have only one server for each component, such as a single web server, application server, and database server. This means that if any of these components fail, the entire infrastructure could go down, leading to service disruption for users. This creates a single point of failure, making the infrastructure vulnerable to potential outages.
2.	Downtime when maintenance is needed: Since there is only one web server in the infrastructure, any maintenance work required, such as deploying new code or updating the server software, will require the server to be taken down. This will result in downtime for users, as they will not be able to access the website while the maintenance work is being carried out.
3.	Cannot scale if too much incoming traffic: If the website experiences a sudden spike in traffic, the infrastructure may not be able to handle the increased load. Since there is only one web server, it cannot handle large amounts of traffic, which could cause the website to become slow or unresponsive, leading to a poor user experience. This inability to scale could result in lost revenue or reduced user engagement.



1. Distributed web infrastructure

For every additional element, why you are adding it - 
•	Two servers for redundancy and to avoid a single point of failure. If one server fails, the other server can continue to handle traffic.
•	A load balancer to distribute incoming traffic across the two web servers, ensuring even traffic distribution and fault tolerance.
•	A web server (Nginx) installed on both servers to serve static content and act as a reverse proxy to forward requests to the application server, improving performance and security.
•	An application server where our codebase is hosted, responsible for processing dynamic content and interacting with the database, separating the application logic from the web server.
•	A database (MySQL) to store and manage application data, ensuring data durability and maintaining data integrity.

2.	What distribution algorithm your load balancer is configured with and how it works: The load balancer (HAproxy) can be configured with various distribution algorithms. One commonly used algorithm is round-robin, where incoming requests are distributed in a cyclic manner to each server in turn. This ensures that each server receives an equal number of requests, distributing the load evenly across the servers.

3.Is your load-balancer enabling an Active-Active or Active-Passive setup, explain the difference between both:
The load balancer in this infrastructure is configured with an Active-Active setup, where both web servers are active and handling requests simultaneously. In an Active-Passive setup, only one server is active and handling requests while the other server is passive and ready to take over if the active server fails. Active-Active setups provide better scalability and higher availability since both servers are handling requests simultaneously.

4.	How a database Primary-Replica (Master-Slave) cluster works: 
In a Primary-Replica (Master-Slave) cluster, one node (the primary) serves as the primary source of truth for data while other nodes (replicas or slaves) replicate data from the primary node. When data is written to the primary node, it is propagated to the replica nodes, ensuring data durability and availability. If the primary node fails, one of the replica nodes can be promoted to become the new primary node, maintaining data integrity.

5. What is the difference between the Primary node and the Replica node in regard to the application? 
The primary node is responsible for handling write requests and maintaining data consistency across all nodes in the cluster. On the other hand, the replica nodes are responsible for handling read requests and replicating data from the primary node. In regard to the application, the primary node is the authoritative source of data, and any updates made to the data are made on the primary node first before being propagated to the replicas. Replica nodes can be used to improve read performance and scalability, but they do not handle write requests directly.



2. Secured and monitored web infrastructure

1.	For every additional element, why you are adding it:
•	Three firewalls are added to the infrastructure to provide an additional layer of security and to monitor traffic between the servers and the internet.
•	An SSL certificate is added to enable HTTPS traffic to secure data in transit between the servers and the users.
•	Three monitoring clients are added to collect performance and usage data from each server to detect and troubleshoot issues proactively.

2.	What are firewalls for: 
Firewalls are security mechanism that acts as a barrier between the servers and the internet, allowing or blocking traffic based on predefined rules.

3.	Why is the traffic served over HTTPS: 
HTTPS encrypts the data in transit between the servers and the users, protecting against eavesdropping, data tampering, and man-in-the-middle attacks. It ensures that sensitive data, such as passwords, credit card information, and personal data, are kept private and secure.

4.	What is monitoring used for: 
Monitoring is used to collect data from the infrastructure, including server performance, usage, and availability, detect issues proactively, troubleshoot problems, and optimize the infrastructure.

5.	How is the monitoring tool collecting data: It is collecting data by installing monitoring clients on each server, which are responsible for collecting performance and usage data from the server's operating system and applications. This data is then sent to the monitoring service, which processes, analyzes, and visualizes the data in real-time, providing insights into the server's health, status, and performance.

6.	Explain what to do if you want to monitor your web server QPS: To monitor your web server QPS (queries per second), you can use a monitoring tool that supports metrics collection from web servers. You can configure the monitoring tool to collect QPS metrics from the web server logs or use an agent to collect metrics from the web server in real-time. Once the metrics are collected, you can set up alerts or dashboards to monitor the QPS and identify any abnormal behavior or performance issues. You can also use the data to optimize the web server's configuration and improve its performance.

issues are with this infrastructure:

1. Why terminating SSL at the load balancer level is an issue: 
Terminating SSL at the load balancer level can be an issue because it can increase the risk of a man-in-the-middle attack. SSL/TLS is used to encrypt the data in transit between the client and the server, but if SSL/TLS is terminated at the load balancer, the data is decrypted and forwarded to the server in plain text, which can be intercepted and read by an attacker. If the SSL/TLS is terminated at the server level, the data remains encrypted until it reaches the server, which increases security.
2. Why having only one MySQL server capable of accepting writes is an issue: 
Having only one MySQL server capable of accepting writes can be an issue because it can create a single point of failure. If the MySQL server fails, the application will no longer be able to write data to the database, which can result in data loss, decreased availability, and potential service interruptions. To increase availability and resilience, it's recommended to have multiple MySQL servers configured in a cluster with replication, so that data can be written to any node and replicated to other nodes, providing failover and automatic recovery.
3. Why having servers with all the same components (database, web server, and application server) might be a problem: 
Having servers with all the same components can be a problem because it can increase the risk of a single point of failure and limit scalability. If all servers have the same components, such as the same web server, application server, and database, a failure in any of these components can affect the entire infrastructure, leading to downtime and service interruptions. To increase resilience and scalability, it's recommended to have multiple servers with different roles, such as load balancers, web servers, application servers, and database servers, distributed across multiple data centers and regions. This approach can provide redundancy, load balancing, and fault tolerance, and enable horizontal scaling.

3. Scale up

here are the reasons why each additional element is added:
1. Load balancers: 
Load balancers are added to distribute incoming traffic between the web server and application server, providing load balancing and failover in case one of the servers fails. By adding two load balancers configured as a cluster, we ensure high availability and scalability of the infrastructure.
2. Web server: 
A dedicated web server is added to handle HTTP requests and serve static content. This allows us to optimize the server for serving static content and free up resources on the application server for processing dynamic content.
3. Application server: 
A dedicated application server is added to handle dynamic content and application logic. This allows us to optimize the server for processing dynamic content and provide more resources for processing complex application logic.
4. Database server: 
A dedicated database server is added to store and retrieve data. This provides better performance, scalability, and security compared to running the database on the same server as the application or web server. Additionally, having a separate database server allows us to configure the database for optimal performance and security.
5. Clustered load balancer: 
Adding a clustered load balancer provides redundancy and high availability. In case one of the load balancers fails, the other load balancer can take over the traffic seamlessly, ensuring that the infrastructure is always available.
6. Split components: 
By splitting the components across multiple servers, we ensure better security, scalability, and performance. Each component can be optimized for its specific role, and any vulnerabilities or issues in one component do not affect the others.
7.Firewall: 
Firewalls are added to provide network security and prevent unauthorized access to the infrastructure. By configuring the firewalls to only allow necessary traffic, we reduce the risk of attacks and ensure that the infrastructure is secure.
8. SSL certificate: 
An SSL certificate is added to provide encryption for web traffic, ensuring that user data is transmitted securely. This improves the security of the infrastructure and builds trust with users.
9. Monitoring clients: 
Monitoring clients are added to collect data about the infrastructure and alert administrators in case of issues. By monitoring key metrics such as CPU usage, memory usage, and network traffic, we can identify and resolve issues before they impact users.

