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
    * [Test Scenario](#test-scenario)

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

## Test Framework
<img src="../images/perf/perf-test-framework.png">

## Test Tools
* [kubemark](https://github.com/kubernetes/kubernetes/tree/master/test/kubemark)
* [fortio](https://github.com/fortio/fortio)

## Metrics Tools
* [Prometheus](https://github.com/prometheus/prometheus)
* [Grafana](https://github.com/grafana/grafana)

## Test Scenarios

### Test Scenario
<img src="../images/perf/perf-app-deploy.png">
