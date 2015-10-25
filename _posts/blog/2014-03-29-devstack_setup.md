---
layout: post
title: "Setting up DevStack with Neutron"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2014-03-29T08:08:50-04:00
---

Devstack is an All-In-One shell script widely used by OpenStack-devs to build a complete OpenStack development environment. It is advisable to install DevStack on a
virtual machine and take periodic snapshots of the VM. When things go messy, you can easily revert back to a working snapshot!

#### Prerequisites :

1. [Install VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)
2. Download the [minimal ubuntu iso](http://archive.ubuntu.com/ubuntu/dists/precise/main/installer-amd64/current/images/netboot/mini.iso) for Ubuntu 12.04. All other
additional dependencies will be installed during DevStack setup.
3. Create a new VM having a minimum requirement of 2 GB RAM and 20 GB of storage.
4. After the VM is successfully created, run the following commands in terminal :-
   1. $ sudo apt-get update
   2. $ sudo apt-get install git


#### After the initial setup, its time to install DevStack !    

1. Clone the DevStack repository :-$ git clone git://git.openstack.org/openstack-dev/devstack   
2. Switch to the cloned devstack directory :- $ cd devstack  
3. Run the following command :-  devstack$ cp /samples/local.conf local.conf  
4. Open the local.conf file and edit the password fields and add neutron to enabled services      

    ADMIN_PASSWORD=password  
    MYSQL_PASSWORD=password  
    RABBIT_PASSWORD=password  
    SERVICE_PASSWORD=password  
    SERVICE_TOKEN=password    
    
    disable_service n-net  
    ENABLED_SERVICES+=,q-svc,q-agt,q-dhcp,q-l3,q-meta,neutron        

5. Now run the shell script to setup DevStack development environment. devstack$ ./stack.sh  
    <em>The script takes a while to complete.</em>


##### If stack.sh fails with following error :    

    2014-03-28 20:39:57.007 | E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)  
    2014-03-28 20:39:57.009 | E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?  
    2014-03-28 20:39:57.016 | ++ err_trap  
    2014-03-28 20:39:57.018 | ++ local r=100  
    2014-03-28 20:39:57.019 | stack.sh failed: full log in /opt/stack/logs/stack.sh.log.2014-03-28-203740  
  
  

This error usually occurs when the ubuntu package manager is running in background. A workaround for this is :-    
    $ sudo fuser -v /var/lib/dpkg/lock      

##### On successful installation, you ll be prompted to relevant URLs, accounts and passwords!      


#### Running unit tests for neutron :    

The tests folder can be found at /opt/stack/neutron/neutron/. The tests are categorized mainly as :-    

1. Unit Tests - The source code is divided into small modules and each module is tested independently to ensure that the module conforms to its intended behavior.
   Mock objects are used to test the modules in isolation.  
   You could take a look at the [Python Mock Module](http://python-mock.sourceforge.net/).    

2. Functional Tests - These tests describe <em>what</em> the software <em>structure</em> actually does by validating the output based on the given input.      


The tests can be run using the ./run_tests.sh script or using tox or nose . Tox creates a virtual environment for running the tests cases. It uses `Testr`_ for
managing the running of the test cases.

1. /opt/stack/neutron/$ :- ./run_tests.sh -V  
   The script runs all the tests from the root source directory.  
  
2. /opt/stack/neutron/$ :- tox -e py27  
   This runs all the unit tests for python 2.7 version dependencies.  
  
These tests take a lot of time to run!
  
##### <em><i> Individual tests can also be run using the following syntax. :-</em></i>    

./run_tests.sh  neutron.tests.unit.test _wsgi
  
##### Using tox :-  

tox -e py27  neutron.tests.unit.test_wsgi     

The <i>pep8</i> tests can be run using tox as follows:    

tox -e pep8 neutron.tests.unit.test_wsgi  
  
  

