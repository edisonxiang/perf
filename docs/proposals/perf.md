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
creation-date: 2019-03-28
last-updated: 2019-03-28
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

* To design the specific implementation of single performance test.

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

### 1. Edge Nodes join in K8S Cluster
<img src="../images/perf/perf-edgenodes-join-cluster.png">

Test Cases:
* Measure Edge Nodes join in K8S Cluster startup time.

  Different numbers of Edge Nodes need be tested.
  Edge Nodes numbers are one of: [1, 10, 20, 50, 100, 200]

  This test case ends with all Edge Nodes are in `Ready` status.

### 2. Create Device/Device Model from Cloud
<img src="../images/perf/perf-create-device.png">

This scenario is expected to measure the northbound API of KubeEdge.

Test Cases:
* Measure the latency between K8S API Server and KubeEdge Cloud Part.
* Measure the throughput between K8S API Server and KubeEdge Cloud Part.

### 3. Report Device Status to Edge
<img src="../images/perf/perf-report-devicestatus.png">

This scenario is expected to measure the southbound API of KubeEdge.

Test Cases:
* Measure the latency between KubeEdge Edge Part and device.
  Device numbers are one of: [1, 10, 20, 50, 100, 200]
* Measure the throughput between KubeEdge Edge Part and device.
  Device numbers are one of: [1, 10, 20, 50, 100, 200]

As the result of the latency and throughput with different device numbers, we can evaluate Scalability of Devices for KubeEdge Edge Part. Measure the capacity of devices can be supported by KubeEdge Edge Part.
<img src="../images/perf/perf-multi-devices.png">

Different protocols are considered between KubeEdge Edge Part and devices.
E.g. Bluetooth, MQTT, ZigBee, BACnet and Modbus and so on.
Currenly Less than 20ms latency can be accepted in Edge IoT scenario.
We can distinguish two kinds of test cases: emulators of different devices and real devices.

### 4. Application Deployment from Cloud to Edge
<img src="../images/perf/perf-app-deploy.png">

This scenario is expected to measure the performance of KubeEdge from Cloud to Edge.

Test Cases:
* Measure the pod startup time with docker images exist in the Edge Node.
  Edge Nodes numbers are one of: [1, 10, 20, 50, 100, 200]
  Pods numbers per Edge Node are one of: [1, 2, 5, 10, 20]
* Measure the pod startup time without docker images exist in the Edge Node.
  Edge Nodes numbers are one of: [1, 10, 20, 50, 100, 200]
  Pods numbers per Edge Node are one of: [1, 2, 5, 10, 20]

As the result of the pod startup time, we can evaluate Scalability of KubeEdge Edge Nodes.
Measure the capacity of Edge Nodes can be supported by KubeEdge Cloud Part.
<img src="../images/perf/perf-multi-edgenodes.png">

Currently the simulation of generating virtual KubeEdge Edge Nodes are being analyzed with Kubernetes  `Kubemark` framework. The following deployment types of KubeEdge Edge Nodes can be used for test:
* Deploy virtual KubeEdge Edge Nodes in single VM.
* Deploy actual KubeEdge Edge Node per dedicated VM.

### 5. Update Device Twin State from Cloud to Device
<img src="../images/perf/perf-update-devicetwin.png">

This scenario is expected to measure the e2e performance of KubeEdge.

Test Cases:
* Measure CPU and Memory Usage of KubeEdge Cloud Part.
  Edge Nodes numbers are one of: [1, 10, 20, 50, 100, 200]
  Device numbers are one of: [1, 10, 20, 50, 100, 200]
* Measure CPU and Memory Usage of KubeEdge Edge Part.
  Edge Nodes numbers are one of: [1, 10, 20, 50, 100, 200]
  Device numbers are one of: [1, 10, 20, 50, 100, 200]

These test cases should be run in both idle and under heavy load.
