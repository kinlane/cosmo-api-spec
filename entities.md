# Entities

I think it's useful to plan (and understand) the API by first listing the entities that the API exposes. 

The API is REST based with trying to leverage the standard and most common best practices.  
Current snapshot of the API can be found here [http://rantav.github.io/cosmo-api-spec/#!/cloudify.json](http://rantav.github.io/cosmo-api-spec/#!/cloudify.json)

## Blueprint
A blueprint is a specification which includes a set of:

* Workflows
* Topologies
* Monitoring information.
* (What more?)

The specification is described by means of a yaml document.

The API should allow _submitting_ a new blueprint. _modifying_ an existing blueprint, _deleting_ a blueprint, _diffing_ between blueprints etc.

Blueprints are versioned so it is possible to have multiple loaded versions of the same blueprint.

You load a new blueprint by POSTing it's yaml content to the cloudufy server. 

## Workflow
A workflow is part of the blueprint and is used to describe things such as deployment, scaling or undeployment.

A blueprint may have zero or more workflows (correct?)

## Topology
A topology describes a set of nodes and their relationships.

Each blueprint has exactly one topology (correct?)

## Node
A node is a single entity within a topology.
A Node in cloudify might be an actual physical hardware host, a virtual host, a process within a host or a service within a process within a host.

Nodes have relationships between them, so for example a node might be contained within another node or may contain others, might depend on other nodes etc.

Example nodes might be: a host, or a database software etc.

## Relationship
Relationships describe the connections between nodes. 

For example a node might be contained within another node (for example the mysql server is contained in a host called mysql01).

Relationships are part of the Topology

## Execution
An execution is the result of running a single (?) workflow.

The data stored in an execution is the runtime data of running the workflow, e.g. logs, metrics and as well as references to the workflow and topology themselves.
A result of such execution might be a new Deployment, as well as a scaling event of an undeployment event.
(Question: How do we represent those events, such as undeployment?)

## Deployment
A deployment is a (one possible) result of an execution.

An execution takes a workflow and runs it on a topology and by that creates a real live set of servers or softwares that are installed and running. A deployment is the description of those running servers, or live nodes.

It is possible to run a workflow that describes a deployment several times (create a few executions) and by that create a few deployments.

A deployment describes the current state of a set of softwares and servers, including data about their metrics, health status, logs etc.
Each deployment is a result of one of more ordered workflows. So for example a deployment might be created by executing a deployment workflow first, and a few days later, running a scale workflow.

## DeploymentDiff
After deploying (executing a deployment workflow) the state of the softare may change. Crashes, out of band modifications (an admin ssh-ing to the server and messing with it) etc. A DeploymentDiff is what describes this difference - b/w the desired deployment state and the actual deployment state. 
