# Install-Cassandra-Cluster-on-EC2-instance
Let's take a look at how to install Cassandra Cluster on EC2 instance  running Ubuntu 18.04 LTS .
We are simulating installing a Cassandra Cluster in a single data center and a single rack. 
The Seed controls the cluster and is used to bootstrap the other nodes in the Cassandra cluster. 

EC2 type      :  t3.xlarge

Operating System    :  Ubuntu 18.04 LTS

openjdk version : "1.8.0_292"

Cassandra version :3.11.2.

Note: Cassandra is need at least 2GB, even with single-node cluster.

Commmand line:

Step 1.

    sudo apt update  
    
Step 2.    

    sudo apt install openjdk-8-jdk -y  (check version : java -version) 
    
Step 3.  

    sudo apt install apt-transport-https   
    
    echo "deb http://downloads.apache.org/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list    
    curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -   
    sudo apt update  
    sudo apt-get install cassandra   
    sudo apt-get install cassandra-tools
    
when I run 'nodetool status'  get :    
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/nodetool_status.PNG)

Note: If get reply is :

    nodetool: Failed to connect to '127.0.0.1:7199' - ConnectException: 'Connection refused (Connection refused)'.
    
It's a RAM issue, I solved the problem after changing EC2 instance type.

Configuring Cassandra
In order to connect to our Cassandra server we will need to configure it. We will be changing configuration in the main Cassandra configuration file located at /etc/cassandra/cassandra.yaml. Open this file so and make the following changes.

    cluster_name: 'Your Cluster Name'
    
    seeds:"EC2 public_IP"

    listen_address: <EC2 private_IP>
    
    rpc_address: <0.0.0.0>
    
    broadcast_address:<EC2 public_IP>

    broadcast_rpc_address:<EC2 public_IP>

First,we are here change the cluster name to something bettter:(Must put its same name for all servers.)
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/Cluster_Name.PNG)

Then,cluster the first node is our seed node so we will put its EC2 public IPv4 address for this value on all three of our servers.
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/Seed_IP.PNG)

Next,we will need to change the listening address for Cassandra. Set this to the private IP address of your EC2.
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/listen_address.PNG)

Other, we need to set the RPC address ,broadcast address and broadcast RPC address.
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/rpc_address.PNG)
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/broadcast_address.PNG)
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/broadcast_rpc_address.PNG)

Lastly,to clear the data from the default directories :

    cd /var/lib/cassandra/data/system
    
    sudo rm -rf *

Starting our Cassandra Cluster to execute the nodetool command :

    sudo systemctl restart cassandra
    
Now you should be able to execute the nodetool command :

    nodetool status
        
We see our server is (U)p and it's state is (N)ormal.
Now we can start the Cassandra service on the other two nodes.
After the services are up we can run the command again and we should see our cluster is fully up and operational.
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/nodetool_status_2.PNG)
