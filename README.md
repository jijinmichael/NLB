## Network Load Balancer

In the context of Amazon Web Services (AWS), NLB stands for Network Load Balancer. An NLB is a load balancing option provided by AWS that distributes incoming traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, or Lambda functions, in a network. It operates at the transport layer (Layer 4) of the OSI model and is designed to handle high volumes of traffic with low latency.

<p align="center">
<img src="https://github.com/jijinmichael/NLB/assets/134680540/4343ffe6-5889-4cdc-bc22-3bde4fefa87d)"></p>

#### Let's see some advantages of NLB
---
- NLB is designed to handle millions of requests per second with **ultra-low latencies**. It can efficiently distribute traffic across multiple targets, ensuring optimal performance for your applications.

- **It supports Elastic IP addresses**, allowing you to maintain a consistent IP address for your load balancer. This is useful if you have clients or DNS records that rely on a fixed IP.

- **NLB supports source IP preservation**, which means that the original source IP address of the client is passed to the target instances. This is valuable for applications that require visibility of the client's IP address for authentication, logging, or compliance purposes.

Overall, NLB provides a highly scalable, reliable, and high-performance load balancing solution for distributing network traffic across multiple targets in AWS.

#### Let's see some disadvantages of Network Load Balancer when compared to rest of the two
---
- **Complexity:** NLB configuration can be complex compared to other load balancing options in AWS. It requires manual registration of targets, security groups, and target group associations, which can be challenging for users who are new to NLB or those who prefer simpler configuration options.

- **Limited Protocol Support:** NLB operates at the transport layer (Layer 4) of the OSI model and supports only TCP and UDP protocols. If your application requires advanced protocol handling or application layer (Layer 7) features, such as HTTP header inspection or URL routing, you may need to consider using Application Load Balancer (ALB) instead.

- **Higher Cost:** NLB can be more expensive compared to other load balancing options in AWS. While NLB offers high performance and scalability, it comes at a higher price point. The cost primarily depends on factors such as the number of NLB instances, the amount of data processed, and the number of cross-AZ data transfers.

- **Limited SSL/TLS Termination:** NLB does not support SSL/TLS termination. If you require SSL/TLS termination at the load balancer level, you would need to offload the SSL/TLS decryption/encryption to the target instances themselves.

- **Limited Cross-Region Load Balancing:** NLB supports cross-Availability Zone (AZ) load balancing within a single AWS region. However, it does not natively support load balancing across multiple regions.

Let design a small NLB for demonstration.
---

For this create two instances with the following user data for testing purpose. I'm creating instances in two subnets named ap-south-1a and ap-south-1b.
```
#!/bin/bash


echo "ClientAliveInterval 60" >> /etc/ssh/sshd_config
echo "LANG=en_US.utf-8" >> /etc/environment
echo "LC_ALL=en_US.utf-8" >> /etc/environment
systemctl restart sshd.service

yum install httpd php -y

cat <<EOF > /var/www/html/index.php
<?php
\$output = shell_exec('echo $HOSTNAME');
echo "<h1><center><pre>\$output</pre></center></h1>";
echo "<h1><center>shopping-app-version-1</center></h1>"
?>
EOF


systemctl restart php-fpm.service httpd.service
systemctl enable php-fpm.service httpd.service
```
Once the instances is created, go to the **Network & Security** >> **Elastic IPs** >> **Allocate Elastic IP address**.

Please note: Allocate 2 IP's and give them a tag for the IP's.

![image](https://github.com/jijinmichael/NLB/assets/134680540/6d040f4d-9cbe-42c3-aaf5-46d84d8c7945)

![image](https://github.com/jijinmichael/NLB/assets/134680540/dc710303-21a7-402c-af37-9f3aa324c126)

Then create a target group. Go to **Load Balancing** >> **Target Groups** >> **Create target group**.

Please choose a target type : Instances 
Target group name           : shopping-nlb-tg
Protocol                    : TCP 

**Please keep in mind that use TCP as the protocol while using NLB as your load balancer**.

Port                        : 80

<p align="center">
<img src="https://github.com/jijinmichael/NLB/assets/134680540/677151aa-142f-4eaf-90a4-41ae1d5e9fab"></p>

Then select the targets.

<p align="center">
<img src="https://github.com/jijinmichael/NLB/assets/134680540/4caf5785-ec85-4162-b4f2-a134e43c1545"></p>

After creating the TG, create NLB.

Go to **Load Balancing** >> **Load Balancer** >> Select **NLB**, Create.

Load balancer name  : shopping-app

Under mapping section, choose the subnets and use the Elastic IP address which is already allocated in our previous sections.

![image](https://github.com/jijinmichael/NLB/assets/134680540/ac03583e-55da-4738-b0af-1ce47b2724a2)

Then add listener TLS:443 and TCP:80 and assign the created TG to it.

![image](https://github.com/jijinmichael/NLB/assets/134680540/892d4d5d-7579-4ed0-b8c1-d6055f5541d9)

Then load the cer from ACM.

![image](https://github.com/jijinmichael/NLB/assets/134680540/4ff1984d-3109-4ba4-be47-98c90b8b998f)

After this, we need to map the root domain name to the allocated elastic IP's from the Route53. Go to **Route53** >> **DNS management** >> **Hosted zone** >> Select the domain >> Create a record.

Then add the allocated elastic IP's as shown below figure then click on create records.

![image](https://github.com/jijinmichael/NLB/assets/134680540/7561499a-6be1-43fe-9643-5eea80ab81cc)

Once the above record is added, you can access the root domain which is associated with the NLB. 


