# clustering-openstack

# Project Description:
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


#### List the flavors

```
openstack flavor list 
```
![flavor list](https://user-images.githubusercontent.com/19154337/36341891-7f3521c8-13f5-11e8-87c6-442e428046e9.png)

A flavor specifies a virtual resource allocation profile which includes processor, memory, and storage.

#### List the availabel images
```
openstack image list
```
Screenshot: List of images
![image list](https://user-images.githubusercontent.com/19154337/36341806-15b0ee18-13f4-11e8-9381-b9c3e938cec1.png)

We will use CentOS7 or Fedora images

#### List the availabel networks
```
openstack network list
```

![network list](https://user-images.githubusercontent.com/19154337/36341907-ba4099b4-13f5-11e8-9343-19c9f32bb0ee.png)


#### List the created security groups
```
openstack security group list
```
Screenshot: Security groups
![scurity group](https://user-images.githubusercontent.com/19154337/36341856-efd324e4-13f4-11e8-8e61-14c60708e302.png)

#### Create the instance

Example:

```
openstack server create --flavor XXXXX --image XXXXXX  --nic net-id=XXXXXXX --security-group XXXXXX  --key-name XXXXXXX provider-instance
```

With our data:

```
openstack server create --flavor XXXXX --image XXXXXX  --nic net-id=XXXXXXX --security-group XXXXXX  --key-name XXXXXXX provider-instance
```

### Assing IP Floating

How to assign Floating IP to the instance:



### Assign IP (internal)....

How to assing a specific Internal IP (10.....) to the instance.

## Script

The script will allow us to create an instance environment with specific configuration parameters.

Input Parameters:

- Options: start, status and delete. Start: Create a new cluster with a name; Delete: Remove all instances assiciated to the cluster; Status: check the status of the cluster.
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
(https://docs.openstack.org/mitaka/install-guide-ubuntu/launch-instance-provider.html)
