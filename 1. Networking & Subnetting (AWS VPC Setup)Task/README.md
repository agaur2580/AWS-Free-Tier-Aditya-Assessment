Brief explanation of how i designed the VPC and subnets
answer
1 . VPC
I created one main VPC with the CIDR block 12.0.0.0/16.
This range is large and gives enough IP addresses to divide into multiple subnets.
A-16 block allows 65,536 IPs, which is more than enough for a small architecture.
This VPC works as the main private network where all AWS resources will run.

2 . Subnets
Inside the VPC, I created 2 subnets:
1 public subnets
1 private subnets
I divided the main VPC CIDR into /24 ranges because /24 gives 256 IPs per subnet, which is perfect for small setups.
The subnet CIDR ranges I used:
Subnet Type	Name	CIDR Block	Why I chose it
Public Subnet 1	test-public-subnet-a1	12.0.1.0/24	First public block (easy to identify)
Private Subnet 1	test-private-subnet-a1	12.0.2.0/24	First private block
I chose simple sequential ranges so it is easy to understand which subnets belong together.

c) Route Tables
I created two route tables:
1 Public Route Table
Connected to the Internet Gateway (IGW)
Public subnets use this route table
Contains a route:
➜ 0.0.0.0/0 → IGW
This allows EC2 instances in public subnets to access the internet.
2 Private Route Table
Connected to the NAT Gateway
Used by private subnets
Contains a route:
➜ 0.0.0.0/0 → NAT Gateway
This allows private EC2 instances to access the internet only for updates, but they remain unreachable from outside.
d) NAT Gateway + Internet Gateway
Internet Gateway (IGW)
Attached to the VPC
Allows public subnets to communicate with the internet
Necessary for public EC2 instances
NAT Gateway
Placed inside public subnet
Gives private subnets secure outbound internet
Private EC2 can download updates but stay protected from internet traffic 
