# Initialize cluster

With the cio software installed on all nodes, the next step is to configure a cluster and then initialize the cluster for use. As part of cluster creation, cio will automatically discover and add drive resources from each node into a storage pool. Drives that are partitioned or have a file system will not be added.

Start configuring a cio storage cluster with the `cioctl create` command. Example:
```
[root@c1 ~]# cioctl create
Cluster started. The current node is now the primary controller node. To add a storage node to this cluster, run the following command:
    cioctl join 192.168.3.95 root f26e695d

After adding all storage nodes, return to this node and run following command to initialize the cluster:
    cioctl init f26e695d
```
The first node where the `cioctl create` command runs becomes the sds controller node (c1 in example above). This node is identified as the sds node in the `cio node ls` command.

The output of the create sub-command includes a `cioctl join` command to add new nodes to the cluster. Add nodes by running the `cioctl join` command on each new node. 

Example four node cluster with new nodes c2, c3, c4:
```
[root@c2 ~]# cioctl join 192.168.3.95 root f26e695d
Adding this node to cluster as a storage node
[root@c3 ~]# cioctl join 192.168.3.95 root f26e695d
Adding this node to cluster as a storage node
[root@c4 ~]# cioctl join 192.168.3.95 root f26e695d
Adding this node to cluster as a storage node
```
Return to the sds controller node and run the `cioctl init` command to complete initialization of the cluster.
```
[root@c1 ~]# cioctl init f26e695d
cluster: initialization started
...
cluster: Node initialization completed
cluster: Start cio daemon
cluster: Succeed: Add vd0: Type:3-copy, Size:20GB
cluster: MongoDB ready
cluster: Synchronizing VID files
cluster: Starting API
```
Note: If you are testing a single-node cluster, skip the `cioctl join` command and just run the `cioctl init` command.

**Initializing bare metal servers with SSDs**

The initialization process will take a few minutes to complete for virtual servers. However when the cio software is installed on physical servers with high performance devices such as SSDs, the first initialization of the cluster will take about 30 minutes. This extra time is used to characterize the available performance. The performance information is used in the quality-of-service (QoS) feature to deliver guaranteed performance for individual applications. 

**Ready To Use**

If Kubernetes is not detected, the cio software will automatically configure a Docker Swarm cluster. Example below shows a Swarm cluster with three manager nodes and one worker node.
```
[root@c1 ~]# docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
gpx9996b1usy7a0h6cd686g62 *   c1                  Ready               Active              Leader
p917q3v1w3gapqx2zn87652f3     c2                  Ready               Active              Reachable
velj1g30557mhayy1hkoqqc75     c3                  Ready               Active              Reachable
jw4robjsehwzw7en48rw2mjie     c4                  Ready               Active
```
At the end of initialization, a Portainer service is launched to provide an GUI for cluster management. Example:
```
[root@c1 ~]# docker service ps portainer
ID                  NAME                IMAGE                        NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
9jpoaen6ddke        portainer.1         portainer/portainer:latest   c1                  Running             Running 8 minutes ago
```
Login to the Portainer UI by pointing your browser at any node IP and port 9000. You can confirm the node IPs with the `cio node ls` command:
```
[root@c1 ~]# cio node ls
NODENAME             IP                NODE_ID    ROLE       STATUS
c1                   192.168.3.95      4132353b   sds        normal
c2                   192.168.3.53      dceacd20   backup1    normal
c3                   192.168.3.145     9ee22782   backup2    normal
c4                   192.168.3.129     d2004822   standard   normal
```
In this example, point the browser at 192.168.3.95:9000, where 9000 is the default Portainer service port number.