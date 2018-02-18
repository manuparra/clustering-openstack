# clustering-openstack

### Project Description
In this project we pretend to describe the mecanism, used to deploy a cluster on Spark. We gonna describe the 

### Table of Content
1. Introduction
2. OpenStack commands used in this project 
	* [OpenStack command line](#OpenStack-command-line)
	* [Create instances](#Create-instances)
	* [List the flavors](#How-to-show-the-flavors)
	* [List the availabel images](#List-the-availabel-images)
	* [List the availabel networks](#List-the-availabel-networks)
	* [List the created security groups](#List-the-created-security-groups)

3. Scripting
4. Spark configuration


#### Create instances

To launch an instance, we must at least specify the flavor, image name, network, security group, key, and instance name.

We will work with the ``Name`` or ``ID`` of the elements that we will show:

---------------------

#### List the flavors

```
openstack flavor list 
```
![flavor list](https://user-images.githubusercontent.com/19154337/36341891-7f3521c8-13f5-11e8-87c6-442e428046e9.png)

A flavor specifies a virtual resource allocation profile which includes processor, memory, and storage.

------------------------------

#### List the availabel images
```
openstack image list
```
Screenshot: List of images
![image list](https://user-images.githubusercontent.com/19154337/36341806-15b0ee18-13f4-11e8-9381-b9c3e938cec1.png)

We will use CentOS7 or Fedora images

--------------------------------

#### List the availabel networks
```
openstack network list
```

![network list](https://user-images.githubusercontent.com/19154337/36341907-ba4099b4-13f5-11e8-9343-19c9f32bb0ee.png)


#### List the created security groups
```
openstack security group listlaunch a cluster of master and slaves cirtual machines on openstack command line
```
Screenshot: Security groups
![scurity group](https://user-images.githubusercontent.com/19154337/36341856-efd324e4-13f4-11e8-8e61-14c60708e302.png)

------------------------

#### Create the instance

Example:

```
openstack server create --flavor XXXXX --image XXXXXX  --nic net-id=XXXXXXX --security-group XXXXXX  --key-name XXXXXXX provider-instance
```

With our data:

```
openstack server create --flavor 3 --image CentOS7  --nic net-id=55c3bd97-fef8-47cf-bde7-a7f6c22f2d2c --security-group default --key-name rashadkey provider-instance
```
![created instance](https://user-images.githubusercontent.com/19154337/36346610-ec53e78e-1441-11e8-8964-85921835c1b4.png)

--------------------------

### IP Floating
* #### List of Floating IP
```
openstack floating ip list
```
![list of floating ip](https://user-images.githubusercontent.com/19154337/36352310-3cba6cf0-14b7-11e8-8b1e-b9021ffe59cc.png)

For each floating IP address that is allocated to your project, the command outputs the ID of the floating IP 		address, the actual floating IP address, the private IP address of the instance the floating IP address is 		associated with, and the ID for the port that the floating IP address is connected to.

* #### Disassociate floating IP of an instance:
Firstly, let  us see the availabel instances "servers":
![server list](https://user-images.githubusercontent.com/19154337/36357208-05cf519e-14fb-11e8-8854-3bdf3989fc7d.png)

Let's disassociate the floating IP of the instance with the name, CirrOS-cloud-init. We can see that its floating IP is 192.168.10.53. To disassociate it, we do the following:
```
openstack server remove floating ip CirrOS-cloud-init 192.168.10.53
```
Screenshot: Disassociate Floating IP:
![disassociate floating ip](https://user-images.githubusercontent.com/19154337/36357374-6f230e18-14fd-11e8-86ea-c09eae8d57b8.png)

We can see how this floating ip is no longer associated to the specified instance.

* #### Create floating IP 

To associate a floating IP, we can use an exisiting one or create a new one and assign it to our project, before assign it to an instance.
```
openstack floating ip create <network>
```
![create ip floating](https://user-images.githubusercontent.com/19154337/36357950-31cc8ca2-1506-11e8-85a3-42020cf80633.png)

We can see that network in our example, is external.

* #### Associate floating IP to an instance
```
openstack server add floating ip CirrOS-cloud-init 192.168.10.68
```
![associated floating ip](https://user-images.githubusercontent.com/19154337/36358285-81577484-150c-11e8-8500-aac2955ce114.png)
Now we can see how the instance called, CirrOS-cloud-init, has an associated floating IP, and it's, 192.168.10.68; which we created in previous step.

---------------------------------

### Assign IP (internal)....
How to assing a specific Internal IP (10.....) to the instance.

## Script
The script will allow us to create an instance environment with specific configuration parameters.

Input Parameters:

- Options: {start, status and delete} 
	- Start: Create a new cluster with a name;
	- Delete: Remove all instances assiciated to the cluster;
	- Status: check the status of the cluster.
	
- Name of the cluster (identifier of the cluster)
- Number of master nodes
- Number of slave nodes
- IP master node (floating)
- IP slaves nodes (internal)
- Flavor of the set of instances
- Network Name for the instances
- Security group
- Key Name for the instances
- Image for the instances

Parameter Code:

```
import sys
import random,sys,os,math
import argparse
import json


def main():

	parser = argparse.ArgumentParser(description='ClusterOpenStack')

	parser.add_argument('-op','--operation', help='Operation of the cluster', required=True)
	parser.add_argument('-name','--name', help='Name of the cluster', required=True)
	parser.add_argument('-nm','--nummasters', help='Num Masters', required=True)
	parser.add_argument('-ns','--numslaves', help='Num Slaves', required=True)
	parser.add_argument('-ipm','--ipmasters', help='IPs of Masters', required=True)
	parser.add_argument('-ips','--ipslaves', help='IPs of Slaves', required=True)
	parser.add_argument('-fl','--flavor', help='Flavor of the instances', required=True)
	parser.add_argument('-n','--network', help='Network name or ID', required=True)
	parser.add_argument('-s','--security', help='Security name', required=True)
	parser.add_argument('-i','--image', help='Image name', required=True)
	

	args = vars(parser.parse_args())

	print args.operation
	print args.name


main()

```




#### Spark configuration.

#### Bibliograpgy
- Launch instance provider
	* https://docs.openstack.org/mitaka/install-guide-ubuntu/launch-instance-provider.html
- Floating IP
	* https://docs.openstack.org/python-openstackclient/pike/cli/command-objects/floating-ip.html
	* https://help.dreamhost.com/hc/en-us/articles/215912768-Managing-floating-IP-addresses-using-the-OpenStack-CLI
