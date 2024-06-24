# 


![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/59894332-fecd-490f-9487-bae7826ff2cd)



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


## Public Application - Application Load Balancer 

## Server Network Metadata

![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/7a9f5050-9336-4ebb-9f35-dd9226963942)


### Load Balancer Network Configuration

![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/b6c0ea39-b770-4a0b-8d11-dfbef29a4ed6)

![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/ae1c2879-dc07-4c85-ab8c-762123610185)





### Request

* Public IPs are translated at the Internet Gateway on AWS
* Instances and services do not have Public IPs

The communication is divided in 2 phases, the first is between the Customer/Client and the Load Balancer and the second is between the Load Balancer and the Server/Instance (Your Website).

So analyzing the packets we can see, that in the Customer to Load Balancer communication we can see the Customer Public IP, so at this stage can add Geolocation filtering and Ip Filtering (black/white listing), and the Load Balancer Private IP, as stated before, AWS Public IP`s are translated at the IGw.

  - At this stage the controls and security tools that are proitecting at the network level must intercept the traffic going to the customers/internet (0.0.0.0/0) andf the traffic going to the load balancer (10.0.10.201/32 - Specific NIC IP - not recommended as them can change or 10.0.10.0/24 - Public Subnet CIDR/ Load Balancer CIDR that are fix)

In the second stage, we can see the private communication Private Load Balancer IP and Private Instance IP, at this stage the communication is treated as a local communication, so you will be seeing the Load Balancer IP (10.0.10.201/32) and the Server/Instance IP (10.0.20.4/32) in your VPC flow logs.

  - At this stage 

### Response

![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/2aced123-31f2-47bf-b2e1-a8b1c30e59fc)

### Flow Logs

Source to Load Balancer:

![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/8b167fec-f8dc-40ed-9b58-cb78890ac63d)

Load Balancer to Instance:

![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/f63bc1b8-ac44-4349-a053-041a48bf4021)






## Flow Logs Format


![image](https://github.com/VitorCora/CloudNetworking/assets/59590152/3778303a-0833-4c74-b06b-2387c6177053)

