---
title: Performance Test Proposal
authors:
    - "@edisonxiang"
    - "@samy2019"
    - "@pavan187"
approvers:
  - "@qizha"
  - "@CindyXing"
  - "@kevin-wangzefeng"
  - "@Baoqiang-Zhang"
  - "@m1093782566"
creation-date: 2019-03-27
last-updated: 2019-03-27
status: Alpha
---

# Performance Test Proposal

* [Performance Test Proposal](#performance-test-proposal)
  * [Motivation](#motivation)
    * [Goals](#goals)
    * [Non\-goals](#non-goals)
  * [Proposal](#proposal)
    * [Test Framework](#test-framework)
    * [Test Tools](#test-tools)
    * [Metrics Tools](#metrics-tools)
  * [Test Scenarios](#test-Scenarios)

## Motivation

Currently KubeEdge test is focused on automated test suites for unit, integration and e2e test and validation. KubeEdge allows the users to manage large scale of edge nodes, devices from cloud. A set of test specifically performance test can be used to determine the non-functional characteristics of KubeEdge such as latency, throughput, cpu usage, memory usage and so on. As a result, we can also evaluate the future improvement items for KubeEdge.

This proposal lists the possible performance test scenarios and test cases for KubeEdge.


### Goals

* Benchmark the performance against the following Service Level Objectives:
  * Latency: time cost from the moment when the server gets the request to last byte of response sent to the users.
  * Throughput: measure how many requests can be served within given time.
  * Scalability: potential scaling capacity under different load conditions.
  * CPU Usage: measure the cpu usage of KubeEdge under different load conditions.
  * Memory Usage: measure the memory usage of KubeEdge under different load conditions.
* Performance test can be able to run against both dockerized and un-dockerized version of KubeEdge.

### Non-goals

* To design the specific implementation of performance test.

## Proposal

### Test Framework
<img src="../images/perf/perf-test-framework.png">

### Test Tools
* [Kubemark](https://github.com/kubernetes/kubernetes/tree/master/test/kubemark)
* [Fortio](https://github.com/fortio/fortio)

### Metrics Tools
* [Prometheus](https://github.com/prometheus/prometheus)
* [Grafana](https://github.com/grafana/grafana)

## Test Scenarios

### Application Deployment from Cloud to Edge
<img src="../images/perf/perf-app-deploy.png">

Measure Northbound API responsiveness.
Measure the latency, throughput between cloud part and edge part.
E.g. Application Deployment from Cloud to Edge (Also measure pod startup time).
We need to test the nodes with existing images or non-existing images.
For e2e Pod startup latency with image exist in the node: pods and their containers with existing images start within 5s*.
For e2e Pod startup latency without image exist in the node: pods and their containers without image exist start within 10s* (considering the standard image pull from docker hub with specified image size).
Burst API’s : Time to start 100*#nodes pods, measured from test scenario start until observing last Pod as ready.

### Update Device Twin State from Cloud to Device
<img src="../images/perf/perf-update-devicetwin.png">

Measure Northbound and Southbound API responsiveness.
E.g. Syncing Expected Device Twin State between Cloud and Device (Also measure Device Twin State Syncing time).

### Scalability of KubeEdge Edge Nodes
<img src="../images/perf/perf-multi-edgenodes.png">

Measure Scalability: Scaling out the no.of KubeEdge nodes under different load conditions
Measure the capacity of edge nodes can be supported by KubeEdge Cloud Part.

Types of Deployments:

Deployment-Type-1: Deploy K8s Master-VM1, CloudCore-VM2, EdgeCores-VM2

Deployment-Type-2: Deploy K8s Master-VM1, CloudCore-VM2, EdgeCores-VM3

Deployment-Type-3: Deploy K8s Master-VM1, CloudCore-VM2, Each EdgeCores in dedicate VM.

For Deployment-Type-1:
For Type-1 Deployment, We use a dedicated machine for Master setup running(All master components running inside pods). In another machine, we run cloud-core and EdgeCore processes.

Achieving Multiple-Edgecore in Same VM:

Currently when we run the Edgecore, Edgecore will start the 2 handlers while bringing up the edge_core process, which are:
MQTT internal broker on (tcp  0.0.0.0:1884)
Edged Handler (tcp 0.0.0.0:10255)

E2E Framework will have to make sure to disable these handlers while bringing up the edgecore.

E2E framework will use the external MQTT broker for event bus communication, and  disable the Edged Handler (disabling edged handler will have no impact as its only being used for GET pods from podmanager).

And while doing a node registration, Framework will make sure the node register with different node-id for respective edge_core. So that all the edgecore node will be registered to master as a different edge nodes.

For Deployment-Type-2:

For Type-2 Deployment, We use a dedicated machine for Master setup running(All master components running inside pods). In another machine we run cloud-core and dedicated machine for multiple EdgeCore processes.

Achieving multiple edgecore process is same as above deployment scenario.

For Deployment-Type-3:
Currently the simulation of generating Virtual nodes are being analyzed with Kubernetes  “KubeMark” framework.

### Scalability of Devices
<img src="../images/perf/perf-multi-devices.png">

Measure Scalability: Scaling out the no.of Devices per node under different load conditions

Measure the capacity of devices can be supported by KubeEdge Edge Part.
We should consider different protocol test between the edge part and the devices. E.g. MQTT, Bluetoothe, ZigBee, BACnet and Modbus and so on.
Less than 20ms latency can be accepted in edge IoT scenario. Also we can distinguish two kinds of test cases: emulators of different devices and real devices.

### Create Device/Device Model from Cloud
<img src="../images/perf/perf-create-device.png">

Measure the latency, throughput between k8s API Server and cloud part.
Create Device/Device Model from Cloud.
T is time cost.
Measure Latency and Throughput between k8S API Server and KubeEdge Cloud Part

### Report Device Status to Edge
<img src="../images/perf/perf-report-devicestatus.png">

Measure the latency, throughput between the edge part and the IoT devices.


### Edge Nodes join K8S Cluster
<img src="../images/perf/perf-edgenodes-join-cluster.png">

Measure Edge Nodes join K8S Cluster Startup time.

### Measure CPU and Memory Usage of KubeEdge Cloud Part
When the system is both idle and under heavy load and run a long time.
The heavy load need to be defined including the northbound and southbound load.
The running time also need to be defined.

### Measure CPU and Memory Usage of KubeEdge Edge Part
Measure the cpu, memory of KubeEdge Edge Part.
When the system is both idle and under heavy load and run a long time.
The heavy load need to be defined including the northbound and southbound load.
The running time also need to be defined.
