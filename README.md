# Diagnose ssh connectivity problems to an ec2 instance

For someone new to AWS and maybe new to UNIX-based commands as well, setting up a new VPC with an EC2 instance and connecting to that instance via SSH can be challenging. If something is not configured correctly it can be difficult to find and fix the problem.

This course is designed to help someone learn how to investigate and fix potential problems by intentionally creating broken environments in each lab which need to be fixed.

At this time the infrastructure setup is in cloud formation templates, since this was quicker for me to define, test and repeat than using the console. If the structure of the lab is helpful, then it makes sense to invest the effort to describe the same steps in the console, which is where the target audience are likely to setup infrastructure.


## Assumptions

* You have already created an AWS account

* You have at least attempted an introductory course on creating VPCs, subnets, security groups and EC2 instances
  * For example, the [acloud.guru](https://acloud.guru) [Solution Architect Associate Course](https://acloud.guru/learn/aws-certified-solutions-architect-associate])

* You know how to generate and work with SSH keys, including using PuTTY on Windows
  * See [Connecting to Your Linux Instance from Windows Using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)


## Cost

The lab provisions a t2.micro instance which is in the free tier, but it also establishes a VPC flow log to CloudWatch which may incur some small charges.


## Instructions

If you are familiar with GIT, feel free to fork/clone this repository to your local machine in order to access the cloud formation templates.

Otherwise select the *Clone or download* dropdown and *Download ZIP*. Unpack the zip file into a directory on your desktop.

Each step is a lab in its own directory with a README.md file and a cloud formation template in YAML.

You can read the instructions for each lab on [github](https://github.com/davidchatz/aws-diagnose-ec2-ssh) or in your IDE (I use [Visual Studio Code](https://code.visualstudio.com/)), or your preferred text editor. 

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

## Doing each lab

* For the first lab, *create* a new stack and open the template in lab-01

* For each subsequent lab, select the stack created in lab-01 and *update* it by selecting `Actions` -> `Update Stack`. This will modify the existing resources and introduce the problem to be fixed.

* Once you understand the issue and see what errors you see from trying to connect via SSH, update the stack again using the template from lab-01.


## Clean up

To clean up at the end of the course
* Select and delete the stack
* Navigate to `EC2` -> `Key Pairs` and delete `kp-ec2-ssh-lab`

