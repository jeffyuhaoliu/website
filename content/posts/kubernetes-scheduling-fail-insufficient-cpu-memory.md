---
title: "Kubernetes - Scheduling Fail Insufficient CPU and/or Memory"
date: 2018-06-15T23:21:09-07:00
draft: false
---
Encountered this weird issue where after recovering from a serious cluster failure event. The symptoms were that certain **Pending** pods were stuck in that state with errors stating both "Insufficient CPU" and "Insufficient Memory". In my case, these pods were tolerating specific tainted nodes and those nodes had more than enough CPU and memory to schedule the intended pod. 

Turns out the problem was resolved via a restart of the Kubernetes API Servers. For more details, please take a look at https://github.com/kubernetes/kubernetes/issues/45237. According to some people in the comments section of the issue, it looks like the issue is resolved in etcd 3.0.17 (and above? hopefully?). 
