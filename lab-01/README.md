# Lab-01: Build and test the working configuration

The first lab deploys what should be the final working configuration so that we can check that the SSH key is being used correctly.

The environment built by this template includes the following:
- VPC
- Public Subnet
- Route Table
- Network ACL
- Security Group
- EC2 t2.micro Amazon linux instance

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

* Select `Create Stack` -> `Upload a template to Amazon S3` and select the template from this lab

* Enter a name for the stack like `ec2-ssh-lab`

* You can ignore the CIDR ranges for the VPC and subnet

* Select the key you created

* Enter you IP address as the template will lock down SSH to your IP address

* Select `Next` and `Next` and `Create`

* Watch the stack build by selecting the `Events` tab and pressing the refresh button

## Test

* Once the stack has built, select the `Outputs` tab

* If you are on Linux/MacOSX, the SSH command line is displayed to connect to the host

* If you are on Windows, the public IP of the host is also displayed

* Connect to the host using ssh/PuTTY

## Troubleshooting

If you cannot connect to the host, the problem is most likely at your desktop end of the connection.

Check that

* You are specifying the correct path to the SSH key file

* Check you have changed the permissions on the key file so only you can read it

* Check that your firewall is not blocking connections on port 22
