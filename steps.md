```


create vpc >> project-vpc | 10.0.0.0/16

create subnet >> my-subnet | 10.0.1.0/24

instance launch - webserver >> 10.0.1.196

create InternetGateway - my-IG >> Associate to project-vpc

create routetable >> add entry

0.0.0.0/0 - InternetGateway

chmod 400 key.pem
ssh -i "key.pem" ec2-user@54.147.62.76

SG - Inbound - http, ssh
SG - Outbpund
ACL - http - allow

//deletion
- terminate instance
- remove IGW entry from route table
- deassociate internet gateway from vpc
- delete internet gateway
- delete vpc
```
