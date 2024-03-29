## 88. Overview of Load Balancers

***

## 89. Create EC2 Security Groups

* A security group acts as a `virtual firewall` that controls the traffic for one or more instances.

* When you launch an instance, you associate one or more security groups with the instance.

* You `add rules` to `each security group` that allow traffic to or from its associated instances.

* You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group. When we decide whether to allow traffic to reach an instance, we evaluate all the rules from all the security groups that are associated with the instance.

* Services -> EC2 -> Security Groups

### ELB security group (`loadbalancer-sg`)
* Add this Inbound rule
    - HTTP TCP 80 Anywhere
    - copy `loadbalancer-sg Group ID`
  
### Webtier security group (`webtier-sg`)
* Add these Inbound rules
    - HTTP  TCP 80    Custom IP `<loadbalancer-sg Group ID>`
    - SSH   TCP 22    Custom IP
    - MySql TCP 3306  Custom IP `<webtier-sg Group ID>`

***

## 90. Create an ELB Load Balancer

* `EC2` -> Load Balancers -> Create load balancer
    - `Application Load Balancer`
    - name: `standard-lb`
    - http & https
    - default VPC
    - Select two AZ's with one default subnet for each
* configure security groups
    - choose `loadbalancer-sg` security group which we just setup
* configure routing
    - target group: `standard-tg`
    - ping path: /ping
    - allows us to define a "healthy" web server
    - load balancer will only forward to healthy web servers
* register targets
* create

```
$ cd /Users/marshad/Desktop/golang-web-dev/033_aws-scaling/02_load-balancer
$ go mod init 02_load-balancer
$ go mod tidy

$ GOOS=linux GOARCH=amd64 go build -o mybinary
```

```
$ scp -i ~/.ssh/ky-10-24-2023.pem   mybinary    ubuntu@[Public IPv4 DNS]:~/
$ ssh -i ~/.ssh/ky-10-24-2023.pem               ubuntu@[Public IPv4 DNS]

$ sudo chmod 777 mybinary
$ sudo ./mybinary
```

***

## 91. Implementing the Load Balancer

### Create an AMI (Amazon Machine Image)
* EC2 -> Instances -> right-click instance -> create image
    - image name: webserver-2023-10-24
    - description: web server 2023 October 24
    - no reboot: unchecked 
    - allowing your instance to reboot gives a better image
* create image

### Launch a new instance of your AMI in a new availability zone (AZ)
* what AZ is your current instance running in?
    - EC2 -> instances -> look at the availability zone and make note of it
* launch a new instance from your AMI
    - EC2 -> AMIs -> right click -> launch -> next: configure
* subnet: `<choose a different AZ>` -> next: storage -> next tag
    - value: web-server-0002
* security group
    - choose the `webtier-sg` security group we created
* launch
    - specify "key pair" we want the instance to use
* launch instance

### Add new EC2 instance to load balancer's target group
* Add the new instance to the target group
* enter load balancer DNS into a browser to see your load balancer in action
    - refresh your browser to see the switching between web-servers.

***

## 92. Connecting to your MySQL Server using MySQL Workbench

* Services -> RDS
    - `webtier-sg` applied
    - Public Accessible -> No

***

## 93. Hands-on Exercise

***

## 94. Hands-on Solution

```
$ cd /Users/marshad/Desktop/golang-web-dev/033_aws-scaling/04_hands-on/02_solution
$ go mod init 02_solution
$ go mod tidy

$ GOOS=linux GOARCH=amd64 go build -o mybinary
```

***

## 95. Autoscaling & CloudFront

***








