# Lab-01: Build and test the working configuration

The first lab deploys what should be the final working configuration so that we can check that everything works including SSH from your desktop.

The environment built by this template includes the following:
- VPC
- Public Subnet
- Route Table
- Network ACL
- Security Group
- EC2 t2.micro Amazon linux instance
- VPC Flow Logs
- CloudWatch Logs

## Setup

To run each lab:
* Log into your account as a user with the *AdministratorAccess* policy
  * You should not be using the root account, best practise is to create a user with the right level of priviledges 
  * Arguably this user could have a tighter policy than this to complete this lab

* Select the region you prefer to use

* Navigate to `EC2` -> `Key Pairs` -> `Create Key Pair`
  * I'm going to call and refer to this key as `kp-ec2-ssh-lab`, you can use a different name if you like and substitute that name where needed

* Donwload the `kp-ec2-ssh-lab.pem` file into the directory you create for this source
  * Better practise is to copy the file into your `~/.ssh` directory and keep all your key files there

* If on a Linux or MacOSX desktop, change the permissions on this file using `chmod 0600 kp-ec2-ssh-lab.pem`

* If on a Windows host convert the PEM file to PPK using PuTTYgen

* Create a new Key Pair for this lab that you can delete when complete

* Navigate to CloudFormation in the console.

## Build

This stack should take less than 2 minutes to deploy.

* Select `Create Stack` -> `Upload a template to Amazon S3` and select the template from this lab

* Enter a name for the stack like `ec2-ssh-lab`

* You can ignore the CIDR ranges for the VPC and subnet

* Select the key you created

* Enter you IP address as the template will lock down SSH to your IP address

* Select `Next` and `Next` and `Create`

* Watch the stack build by selecting the `Events` tab and pressing the refresh button

## Test

Now that the instance has been deployed, make sure that you can SSH to the host.

* Once the stack has built, select the `Outputs` tab

* If you are on Linux/MacOSX, the SSH command line is displayed to connect to the host

* If you are on Windows, the public IP of the host is also displayed

* Connect to the host using ssh/PuTTY

## Monitoring

The template also sets up flow logs for the VPC. So navigate to `CloudWatch` -> `Logs`
and you should see a log group for this lab. Depending on what you called your stack,
it will be called `loggrp-vpc-ec2-ssh-lab`.

It can take several minutes for AWS to provision the flow log so you may have to wait
before you see anything in that log group. Once it is established you should see an
entry for the elastic network interface (ENI) and when you select that, a log of 
network activity to your VPC.

This feed is not in real time, it will take several minutes to refresh with the latest
data. What you should see is traffic from your desktop to port 22 on your instance and
traffic from port 22 on your instance back to your desktop as ACCEPTED. Note that port
your desktop sends and receives that traffic is a port number above 1024 and will
vary with every connection you make. 

You will also see traffic from other external sources and maybe from the instance itself
to other services being REJECTED. This is why it is important to understand what services
are needed on your instances and to lock down your security groups and NACLs.

Enter your desktop IP address in the search so that you can just your traffic to the instance.

If you are unfamiliar with the flow logs format, see this [document](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html#flow-log-records).


## Review

In the console you should look at how each component is configured, including:

* VPC `vpc-ec2-ssh-lab`
  * Note that the template does not use the default route table and nacl defined with the VPC.
  * TODO: Consider changing the template so it does.
* Subnets `sn-ec2-ssh-lab`
* Route Table `rt-ec2-ssh-lab`
* Internet Gateway `ig-ec2-ssh-lab`
* NACL `nacl-ec2-ssh-lab`
* Security Group `secgrp-ec2-ssh-lab`
* EC2 Instance `inst-ec2-ssh-lab`

## Troubleshooting

If you cannot connect to the host, the problem is most likely at your desktop end of the connection.

Check that...

* You are specifying the correct path to the SSH key file

* You have changed the permissions on the key file so only you can read it

* You are logging in as ec2-user

* Your firewall is not blocking connections on port 22

Any issues need to be resolved before proceeding with the other labs.