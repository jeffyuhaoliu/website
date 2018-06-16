---
title: "Kubernetes - Bad Nodes Scaling Up"
date: 2018-06-15T22:46:36-07:00
draft: false
---
Encountered a very serious Kubernetes issue that almost led to a full cluster outage. The symptoms were the following:

- Kube API Server sometimes become unresponsive and from time to time returned the error message: `Error from server: client: etcd cluster is unavailable or misconfigured; error #0: unexpected EOF`
- Many pods were stuck on **Pending** or **Terminating** state
- etcd CPU and network traffic spiked up dramatically from normal levels

The diagnosis of the problem was that we had to increase our cluster capacity by 50% for a large batch job and new nodes (they had a minor version update) created in the cluster seems to have a yet to be unidentified issue that was causing kubelet on these nodes to throw lots and lots of errors. I suspect that these errors flooded the API Server with messages and requests causing a cascade of failures on nodes that previously worked. 

Unfortunately, the solution to the problem was to terminate the new nodes and eventually the cluster recovered itself (mostly...). For more information about the next issue I encounted trying to full recover pods that were stuck on **Pending**, please see [here]({{< relref "kubernetes-scheduling-fail-insufficient-cpu-memory.md" >}}).

