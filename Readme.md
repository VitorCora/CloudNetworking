# 


![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/7000312a-89f7-4710-83a8-ef2d272c5c3c)


NAT Gateway Communication

| Subnet Name  | Subnet CIDR |
| ------------- | ------------- |
| Public Subnet 1A | 10.0.0.0/24 |
| Load Balancer Subnet 1A  | 10.0.10.0/24  |
| Private Subnet 1A| 10.0.20.0/24  |
| Public Subnet 1B | 10.0.1.0/24 |
| Load Balancer Subnet 1B  | 10.0.11.0/24  |
| Private Subnet 1B| 10.0.21.0/24  |


| Service Name | Service IP |
| ------------- | ------------- |
| NAT Gateway 1A | 10.0.0.45/24 |
| Balancer ENI 1A  | 10.0.10.96/24  |
|Service 1A| 10.0.20.5/24  |
| NAT Gateway 1B | 10.0.0.86/24 |
| Balancer ENI 1B  | 10.0.10.34/24  |
|Service 1B| 10.0.20.1/24  |

## NAT Gateway Communication

### Request

![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/b54f3dea-ebe8-4669-aa04-ac69bcfeb98b)

* Public IPs are translated at the Internet Gateway on AWS
* Instances and services do not have Public IPs

The communication is divided in 2 phases, the first is between the host (Instance or service) and the NAT Gateway and the second is between the NAT Gateway and the Internet Gateway (Public Website).

So analyzing the packets we can see, that the instance to Web Service communication seems to be direct, but as the Service does not have a public IP, it needs to go through a NAT Gateway, that lends its Public IP to this communication and makes this Private service invisible to the public Internet

It is made possible through the addition of a default gateway route to the NAT Gateway (0.0.0.0/0 -> NAT Gw 1A) 

Some real lab screenshots:

On the Instance:
![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/625901ee-d64b-41e3-8fc3-2bdb138aade1)

On the VPC Flowlogs (CloudWatch):
![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/c07a919c-4619-4a95-8cf9-4d3c07606460)

On the Route Table:
![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/651f5482-3860-4c10-9c0e-16ef4bc19f7a)




### Response

![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/2aced123-31f2-47bf-b2e1-a8b1c30e59fc)

