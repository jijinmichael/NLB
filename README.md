### Network Load Balancer

In the context of Amazon Web Services (AWS), NLB stands for Network Load Balancer. An NLB is a load balancing option provided by AWS that distributes incoming traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, or Lambda functions, in a network. It operates at the transport layer (Layer 4) of the OSI model and is designed to handle high volumes of traffic with low latency.

<p align="center">
<img src="https://github.com/jijinmichael/NLB/assets/134680540/4343ffe6-5889-4cdc-bc22-3bde4fefa87d)"></p>

NLB is designed to handle millions of requests per second with ultra-low latencies. It can efficiently distribute traffic across multiple targets, ensuring optimal performance for your applications.

It supports Elastic IP addresses, allowing you to maintain a consistent IP address for your load balancer. This is useful if you have clients or DNS records that rely on a fixed IP.

NLB supports source IP preservation, which means that the original source IP address of the client is passed to the target instances. This is valuable for applications that require visibility of the client's IP address for authentication, logging, or compliance purposes.

Overall, NLB provides a highly scalable, reliable, and high-performance load balancing solution for distributing network traffic across multiple targets in AWS.

Let's see some disadvantages of Network Load Balancer when compared to rest of the two.

- **Complexity:** NLB configuration can be complex compared to other load balancing options in AWS. It requires manual registration of targets, security groups, and target group associations, which can be challenging for users who are new to NLB or those who prefer simpler configuration options.

- **Limited Protocol Support:** NLB operates at the transport layer (Layer 4) of the OSI model and supports only TCP and UDP protocols. If your application requires advanced protocol handling or application layer (Layer 7) features, such as HTTP header inspection or URL routing, you may need to consider using Application Load Balancer (ALB) instead.

- **Higher Cost:** NLB can be more expensive compared to other load balancing options in AWS. While NLB offers high performance and scalability, it comes at a higher price point. The cost primarily depends on factors such as the number of NLB instances, the amount of data processed, and the number of cross-AZ data transfers.

- **Limited SSL/TLS Termination:** NLB does not support SSL/TLS termination. If you require SSL/TLS termination at the load balancer level, you would need to offload the SSL/TLS decryption/encryption to the target instances themselves.

- **Limited Cross-Region Load Balancing:** NLB supports cross-Availability Zone (AZ) load balancing within a single AWS region. However, it does not natively support load balancing across multiple regions.

