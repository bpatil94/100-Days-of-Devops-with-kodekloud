# Day #50 task: Set Resource Limits in Kubernetes Pods
  The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:


Create a pod named httpd-pod with a container named httpd-container. Use the httpd image with the latest tag (specify as httpd:latest). Configure the following container-level resource requests and limits for the container:

Requests: Memory: 15Mi, CPU: 100m

Limits: Memory: 20Mi, CPU: 100m

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


<img width="595" height="407" alt="image" src="https://github.com/user-attachments/assets/09041b3b-711f-4ee7-a989-ffeccc6a0dcd" />


## Resource limits in Kubernetes prevent a container from consuming excessive CPU or memory and affecting other workloads on the node. Resource requests are used by the scheduler to decide where the Pod should run, while limits define the maximum resources the container can consume. CPU exceeding its limit is throttled, whereas exceeding the memory limit can result in the container being OOMKilled.

```
                 Kubernetes Scheduler
                         |
                  Reads requests
                         |
                         v
                 Selects a node
                         |
                         v
                    Pod starts
                         |
              +----------+----------+
              |                     |
          CPU limit             Memory limit
          500m                  512Mi
              |                     |
          Throttling             OOMKill
          if exceeded           if exceeded
```

## Step 1 — Create YAML File
```
vi httpd-pod.yaml
```
<img width="407" height="128" alt="image" src="https://github.com/user-attachments/assets/7b0b4cb8-36b9-44e3-a222-c34e0caffde9" />

## Step 2 — Define Pod Configuration
- Insert the content
- <img width="313" height="251" alt="image" src="https://github.com/user-attachments/assets/c55f01a8-b1c5-49f9-9aa0-dc12059481a2" />


## Step 3 — Apply Configuration
```
kubectl apply -f httpd-pod.yaml
```
<img width="554" height="79" alt="image" src="https://github.com/user-attachments/assets/5ebf4842-0a83-4f43-ba79-660174fc5fb9" />

## Step 4 — Verify Pod Status
```
kubectl get pods
```
<img width="416" height="50" alt="image" src="https://github.com/user-attachments/assets/0ec2bf85-8c8e-48e2-adc4-4827ef7a27aa" />

## Step 5 — Inspect Resource Allocation
```
kubectl describe pod httpd-pod.yaml
```
<img width="884" height="323" alt="image" src="https://github.com/user-attachments/assets/f0558854-fa6a-4a20-a46b-8a41491ed849" />


**💡 Real-World Importance**

  This task reflects real DevOps practices:

   1. Ensures fair resource distribution
   2. Improves application reliability
   3. Prevents cluster crashes
   4. Enables efficient scaling
   5. In production systems, every container typically has defined resource limits to maintain system stability.


**🎯 Conclusion**

  Setting resource requests and limits is a fundamental Kubernetes skill for DevOps engineers.
  By completing this task, you’ve learned how to:

1. Control container resource usage
2. Write production-ready YAML configurations
3. Prevent performance issues in a cluster
