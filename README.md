# Secure VPC Architecture on AWS

## Project Overview

This project demonstrates how to build a secure and isolated network architecture using Amazon VPC.

The architecture contains:
- One VPC
- One Public Subnet
- One Private Subnet
- Internet Gateway
- Public and Private Route Tables
- Security Groups
- Custom Network ACL
- Public EC2 Web Server
- Private EC2 Application Server

## Architecture

Internet
   |
Internet Gateway
   |
Secure-VPC (10.0.0.0/16)
   |
   +-----------------------------+
   |                             |
Public Subnet              Private Subnet
10.0.1.0/24                10.0.2.0/24
   |                             |
Public EC2                 Private EC2
Web Server                 Application Server

## AWS Services Used

- Amazon VPC
- Amazon EC2
- Internet Gateway
- Route Tables
- Security Groups
- Network ACL

## Network Configuration

### VPC
- Name: Secure-VPC
- CIDR: 10.0.0.0/16
- Region: Asia Pacific (Mumbai)

### Public Subnet
- Name: Public-Subnet
- CIDR: 10.0.1.0/24
- Availability Zone: ap-south-1a
- Internet access: Yes

### Private Subnet
- Name: Private-Subnet
- CIDR: 10.0.2.0/24
- Availability Zone: ap-south-1a
- Direct internet access: No

## Route Tables

### Public Route Table

Destination:
0.0.0.0/0

Target:
Internet Gateway

### Private Route Table

The private route table does not contain a direct Internet Gateway route.

## Security Groups

### Public-Web-SG

Inbound:
- SSH (22) - My IP
- HTTP (80) - 0.0.0.0/0

### Private-App-SG

Inbound:
- SSH (22) - Public-Web-SG
- TCP 8080 - Public-Web-SG

This prevents direct access to the private server from the Internet.

## Network ACL

Custom Network ACL:
- Secure-Private-NACL

The NACL is associated with the Private Subnet.

Inbound traffic allows:
- SSH 22 from Public Subnet
- TCP 8080 from Public Subnet

Outbound traffic allows required ephemeral ports for return traffic.

## EC2 Instances

### Public EC2

Name:
Secure-VPC-Public-EC2

Purpose:
Public web server.

Apache HTTP Server was configured and tested successfully.

### Private EC2

Name:
Secure-VPC-Private-EC2

Private IP:
10.0.2.253

Purpose:
Private application server.

The application server runs on TCP port 8080.

## Connectivity Test

The private server was tested from the Public EC2 using:

```bash
curl http://10.0.2.253:8080


Successful response:

Private EC2 Server

This server is inside the Private Subnet.

This confirms connectivity between the Public EC2 and Private EC2 while keeping the Private EC2 without a public IP.


## Public Web Server Test

The Public EC2 web server was accessed through its public IPv4 address.

The website displayed:

Secure VPC - Public Web Server

This confirms that the Public EC2 is accessible through the Internet Gateway.



## Security Design

The Private EC2 instance does not have a public IP address.

Internet-facing traffic is allowed only to the Public EC2.

The Private EC2 can be accessed from the Public EC2 according to the configured Security Group and Network ACL rules.

This architecture provides network isolation and controlled communication between the public and private resources.



## Screenshots

The following screenshots demonstrate the implementation:

1. VPC Configuration
2. Public Subnet
3. Private Subnet
4. Internet Gateway
5. Public Route Table
6. Private Route Table
7. Security Groups
8. Public EC2
9. Private EC2
10. Private EC2 Connectivity Test
11. Public Web Server
12. Network ACL
13. VPC Resource Map


## Conclusion

This project demonstrates a secure AWS VPC architecture using public and private subnets.

The Public EC2 acts as the web server, while the Private EC2 is isolated from direct Internet access.

Security Groups and Network ACLs are used to control network traffic between the resources.

The project successfully demonstrates network isolation and controlled communication in AWS.












