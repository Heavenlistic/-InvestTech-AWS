To deploy 9 resources (VPC, Subnet, Internet gateway, Internet Gateway attachment, Route Table, Route table association, Security Group, Key Pairs, EC2, and SSH to EC2) in AWS using CLI

1. AWS Services > IAM > Users > iamadmin > Security credentials > Access keys : create access key > Command Line Interface (CLI) > Next > Create Access Key > Download

2. Or create Access keys on CLI


Commands 
1. # (to create access keys on CLI) 
aws iam create-access-key --user-name <username> 

2. # (to create a new VPC, this displays a VPC ID) 
aws ec2 create-vpc --cidr-block 172.16.0.0/16 --query Vpc.VpcId --output text 

3. # (to add a subnet, this displays a subnet ID)
 aws ec2 create-subnet --vpc-id <put-the-vpc-id-here> --cidr-block 172.16.1.0/24 --query Subnet.SubnetId --output text 

4. # (to create an Internet Gateway, this displays the igw ID )
aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text (to create an Internet Gateway, this displays the igw ID )

5. # (to attach the Internet Gateway with the VPC)
aws ec2 attach-internet-gateway --vpc-id <vpc-id> --internet-gateway-id <internet-gateway-id>

6. # (to create a Route Table, this displays the rtb ID)
aws ec2 create-route-table --vpc-id <vpc-id> --query RouteTable.RouteTableId --output text

7. # (to add a route)
aws ec2 create-route --route-table-id <route-table-id> --destination-cidr-block 0.0.0.0/0 --gateway-id <internet-gateway-id>

8. # (to associate the Route Table with the Subnet)
aws ec2 associate-route-table --subnet-id <subnet-id> --route-table-id <route-table-id>

9. # (to create a new Security Group)
aws ec2 create-security-group --group-name EC2SecurityGroup --description "Demo Security Group" --vpc-id <vpc-id>

10. # (to open the SSH port(22) in the ingress rules)
aws ec2 authorize-security-group-ingress --group-id <security-group-id> --protocol tcp --port 22 --cidr 0.0.0.0/0

11. # (to create a Key Pair to enable connection via SSH) 
aws ec2 create-key-pair --key-name EC2KeyPair --query "KeyMaterial" --output text > EC2KeyPair.pem

12. # (to grant permission to the key pair)
chmod 400 EC2KeyPair.pem

13. # (to deploy the EC2 instance)
aws ec2 run-instances --image-id ami-087c17d1fe0178315 --count 1 --instance-type t2.micro --key-name EC2KeyPair  --security-group-ids <security-group-id> --subnet-id <subnet-id> --associate-public-ip-address --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=Demo-EC2}]'

14. # (to get ip address)
aws ec2 describe-instances --query "Reservations[*].Instances[*].{InstanceID:InstanceId,State:State.Name,Address:PublicIpAddress}" --filters Name=tag:Name,Values=Demo-EC2

15. # (to SSH into the EC2)
ssh -i "EC2KeyPair.pem" ec2-user@<ec2-public-ip>

16. # (to update with latest application)
sudo yum update -y

17. # (to install apache webserver which deploys appliations)
sudo yum install -y httpd.x86_64

18. # (to start the apache service)
sudo systemctl start httpd.service 

19. # (to enable the apache service & keep it active)
sudo systemctl enable httpd.service. 

20. AWS > EC2 > Instance > Copy & paste Public ips address in browser (displays error message)

21. # Open a new VS Code bash 
aws configure

22. # (to open the port(80) in the ingress rules for http)
aws ec2 authorize-security-group-ingress --group-id <security-group-id> --protocol tcp --port 80 --cidr 0.0.0.0/0

23. # (to open the port(443) in the ingress rules for https)
aws ec2 authorize-security-group-ingress --group-id <security-group-id> --protocol tcp --port 443 --cidr 0.0.0.0/0

24. AWS > EC2 > Instance > Copy & paste public ip address in a different browser (displays the message)

25. # (tells more about the instance)
aws ec2 describe-instances

26. # (to delete the instance)
aws ec2 terminate-instances --instance-id <instance-id> 

27. # (to delete the VPC)
AWS Services > VPC > Actions > Delete VPC > Delete

28. # (to delete the Key pairs)
AWS Services > Key pairs Actions > Delete

29. git init > git add .gitignore > git status > git commit -m "Initial push" > git add . > git commit -m "Initial push" (first delete all previous .git folders to avoid error)

30. git remote add origin (git link) 
    git branch -M main 
    git push -u origin main. 
    
31. git add . > git commit -m "Second push" > git push (for additional changes with updated comments)