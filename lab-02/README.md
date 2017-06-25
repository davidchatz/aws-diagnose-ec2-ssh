# Lab-02: Network ACL

A network access control list (NACL) defines what traffic can go in or out of
the associated subnets.

It is stateless so it does not know which traffic is part of the same 
connection, so both inbound and outbound rules have to be defined.

A trap to watch out for is although the SSH connection by default will
connect to the instance on port 22, the port on your desktop will be a port
number between 1024 and 65535, so the outbound rule needs to be for a broad
range on unprivileged ports. 

## No allow rules

The first [lab-02 template](lab-02-a.yaml) removes the allow rules from the
NACL leaving only the default deny rules.

Update the stack using this template, taking note of the set of changes
CloudFormation is going to apply. In the console review the inbound
and outbound rules in the NACL `nacl-ec2-ssh-lab`.

Once the update is complete attempt to reconnect to your instance using
ssh. In CloudWatch you will see this flagged as REJECTED.


## Inbound only open rule

The [second template](lab-02-b.yaml) adds an open (all traffic) inbound
allow rule to the NACL. 

Apply this update and attempt to connect via SSH again. Despite your
SSH connection request reaching your instance, the return packets are
blocks. In CloudWatch you will see the traffic from your host marked as
ACCEPT, but the traffic from the instance back to your desktop is mrked
as REJECT.

Note that your SSH client reports the same error, it has no way of
knowing that the connection was partially successful.


## Inbound and outbound open rules

The [third template](lab-02-c.yaml) reinstates both the inbound and
outbound open rules. You should now be able to connect again.


## Inbound and outbound port 22 rules

You might consider for your network that you want more string access
controls at this level, especially if you know that nothing in this
instance should accept and send traffic on specific ports or targets.

However locking down a NACL too hard so this it impacts services you
require can be frustrating and hard to resolve. For a service like SSH
a security group for all hosts that you can SSH too might be easier
to manage and have the same effect.

The [fourth template](lab-02-d.yaml) attempts to do this but gets it
wrong by locking down the outbound traffic to port 22 only as well.

After updating to this template you should see SSH connections fail
and CloudWatch report incoming ACCEPT but outgoing REJECT traffic.


## Inbound port 22, outbound unprivileged port rules

The [final template](lab-02-e.yaml) fixes the outbound rule to be a
range of ports.

This update will restore SSH access, but no other incoming traffic
can get to the instance, which is unlikely to be your long term
plan.

## Restore

Update the stack one last time using the [working template](../lab-01/lab-01.yaml).

