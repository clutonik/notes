# Docker Swarm

To test out docker swarm features or learn docker swarm, you can use below website(playground)
which allows you to run a swarm cluster on cloud for free (for 4 hours). All you need is a docker
account. 

[Click here to open playground](https://labs.play-with-docker.com/)

You can also use swarm visualizer while you are learning docker swarm to look at swarm cluster through a GUI.
Command to start swarm visualizer:

```docker service create --name=viz --publish=8080:8080/tcp --constraint=node.role==manager --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock bretfisher/visualizer```

## Contents

- [General](#General)
- [Manager](#Manager)
- [Worker](#Worker)
- [Routing Mesh](#Routing-mesh)
- [Commands](#Commands)

### General

- Any node (manager/worker) can be promoted or demoted to other roles.
- Replicas are also known as tasks. 
- When you initialize docker swarm:
    - docker automates security stuff like root SSL certificate for first node, join token etc.
    - creates Raft database to store root CA, configs and secrets which is encrypted by default on disk.
    - Replicates logs amongst Managers via mutual TLS in "control plane".
- When we want to change a property of a container e.g. changing CPU limits, swarm allows us to make rolling updates which will make changes to one container at a time.

### Manager

- Controls the services/stacks running in a swarm cluster.
- Maintains a local database called Raft to store state information (consensus)
- can act as worker and manager node both and host services.

### Worker

- 


### Routing Mesh

- Routing mesh creates a VIP for each service wich is available for all worker 
nodes and forwards traffic to all nodes hosting a specific service.
![Routing mesh](images/swarm-mesh.png)

- Even if you hit your application port using localhost on a worker node, it might return
response from a container running on another worker node. 
- This is Layer 3 Load balancing done by swarm.
- Docker Enterprise edition comes with Layer 4 Load balancing available to be used.

### Commands

All commands listed below use ```--detach False``` by default, so these commands
wait synchronously until all tasks have been executed. 
```Notes:``` 
- If you are automating stuff with swarm, use ```--detach True``` flag
- All these commands can only be executed at manager node.

- To get list of services running in a swarm cluster:
    - ```docker service ls ```
- To check individual containers running in swarm:
    - ```docker service ps <service-name>```
- To check list of nodes in swarm and their role:
    - ```docker node ls```
- To update a service configuration:
    - ```docker service update <service-name-or-id> <OPTIONS>```
    - e.g: ```docker service update nginx --replicas 3```
- To get the join token for adding a node as manager:
    - ```docker swarm join-token manager```
- 