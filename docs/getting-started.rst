Status of this document: draft 2016-07-21

The "consul" binary is a monolitic application and the way it acts is all set up by config files or command line options.

This role will assist you to configure the needed servers to build up a working consul cluster.

Every node in a consul cluster runs an agent  (An agent is the long running daemon on every member of the Consul cluster)
An agent can be either configured as a server or a client.

Client - A client is an agent that forwards all RPCs to a server. The only background activity a client performs is taking part in the LAN gossip pool. For that reason we need to make sure every client can access all other nodes on the  {{ consul__agent_serf_lan_allow }} network we define.

Server - A server is an agent with an expanded set of responsibilities including participating in the Raft quorum, maintaining cluster state, responding to RPC queries, exchanging WAN gossip with other datacenters, and forwarding queries to leaders or remote datacenters.
We could run a cluster with only a single server, but thats would break the cluster in an outage of the server node. It is suggested to have at least 3
server nodes in a SERF_LAN segment and the amount of server nodes should be always an odd number (to avoid split brain situations in case of outages).
This role can be used to test consul with only 1 server node, but this has to be enabled in a playbook variable. We assume you use the default of 3 servers for a start.


An overview of the consul communications: https://www.consul.io/assets/images/consul-arch-5d4e3623.png

For a more detailed info about consul please look at https://www.consul.io/docs/internals/architecture.html

A main design decision of this role is to split the configuration options into small config file snippets and avoid huge unreadable blocks of configurations. Whenever possible we use default values defined in /defaults/main.yml that can be redefined in your playbooks.

The general config structure will look like:

/etc/consul/agent.d/  	
                         agent.yml 		<- basic agent configuration common to all nodes in the cluster/datacenters
                         tls.yml   		<- if tls is used the config options will be in this file
                         update.yml     <- configure how to check for updates
           /service.d/	           		<- if there are services defined every service will have his config file in this folder
           /checks.d/				    <- tbd
           /watches.d/ 				    <- tbd

In our systemd we make sure the config snippets from the above folder are read from all the folders. Be aware that config files are parsed by alphanumetic order. Later files overwrite the info from previous loaded files.

