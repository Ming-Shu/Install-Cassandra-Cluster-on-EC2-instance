# Install-Cassandra-Cluster-on-EC2-instance
Let's take a look at how to install Cassandra Cluster on EC2 instance  running Ubuntu 18.04 LTS 

EC2 type      :  t3.xlarge

Operating System    :  Ubuntu 18.04 LTS

openjdk version : "1.8.0_292"

Note: Cassandra is need at least 2GB, even with single-node cluster.

Commmand line:

    sudo apt update   
    sudo apt install openjdk-8-jdk -y  (check version : java -version)    
    sudo apt install apt-transport-https    
    echo "deb http://downloads.apache.org/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list    
    curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -   
    sudo apt update  
    sudo apt-get install cassandra   
    sudo apt-get install cassandra-tools
    
when I run 'nodetool status'  get :  
  
![image](https://github.com/Ming-Shu/Install-Cassandra-Cluster-on-EC2-instance/blob/main/nodetool_status.PNG)

Configuring Cassandra
In order to connect to our Cassandra server we will need to configure it. We will be changing configuration in the main Cassandra configuration file located at /etc/cassandra/cassandra.yaml. Open this file so and make the following changes.

    cluster_name: 'Your Cluster Name'
    
    seeds:"EC2 public_IP"

    listen_address: <EC2 private_IP>
    
    rpc_address: <0.0.0.0>
    
    broadcast_address:<EC2 public_IP>

    broadcast_rpc_address:<EC2 public_IP>
