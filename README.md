# CassandraSetupOnAWS

1. Create a new security group by the name "cassandra-ports"
    a) "Custom TCP"and port range of "1024-65500"
    b) "VPC CIDR block" as the allowable traffic source
2. Create 3 EC2 instances (t2-small) using the 7 step workflow
    a) Use the "SKL-Ubuntu-Cassandra" (community AMI) in AZ1, AZ2, AZ3
    b) The AMI contains - Ubuntu 16.04, JDK8, Python2.7 & unconfigured Cassandra
    c) Assign the "cassandra-ports" SG
    d) Download a new PEM file
    e) Open 3 terminal windows, one for each instance
3. Follow the steps to configure Cassandra (repeat this in all AZ)
    a) Go to the "conf" folder and update the cassandra.yaml (reference 1)
    b) Start the cassandra daemon (reference 2)
    c) Issue the nodetool command to ensure that the startup is good
4. When repeating the steps-3 above on the 2 remaining AZ, ensure the slight difference in config
and observe the nodetool command as cassandra forms a distributed ring P2P system!

Command reference - 1
sed -i 's=MOD_CLUSTER_NAME=GL-Cluster=g' cassandra.yaml
sed -i 's=MOD_IP_ADDRESS=[LOCAL IP here]=g' cassandra.yaml
sed -i 's=MOD_SEED_LIST=[SEED IP here]=g' cassandra.yaml
sed -i 's=MOD_DATACENTER=dc1=g' cassandra-rackdc.properties
sed -i 's=MOD_RACK=[RACK 1,2,3]=g' cassandra-rackdc.properties

Command reference - 2
./cassandra
./nodetool status
./nodetool stopdaemon
./cqlsh [EC2 private IP] -u cassandra -p cassandra
