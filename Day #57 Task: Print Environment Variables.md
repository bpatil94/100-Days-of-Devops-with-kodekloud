# Day #57 task: Print Environment Variables
The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.

1. Create a pod named print-envars-greeting.

2. Configure spec as, the container name should be print-env-container and use bash image.

3. Create three environment variables:

  a. GREETING and its value should be Welcome to

  b. COMPANY and its value should be Stratos

  c. GROUP and its value should be Ltd

4. Use command ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"'] (please use this exact command), also set its restartPolicy policy to Never to avoid crash loop back.

5. You can check the output using kubectl logs -f print-envars-greeting command.


Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.




# Step-1: Create the pod manifest file with the given details.
```
vi pod.yaml
```
<img width="427" height="50" alt="image" src="https://github.com/user-attachments/assets/d517cdea-079f-404f-a169-4a99ccf49014" />

- content with task description

```
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
  labels:
    purpose: demonstrate-envars
spec:
  containers:
  - name: print-env-container
    image: bash:latest
    env:
    - name: GREETING
      value: "Welcome to"
    - name: COMPANY
      value: "Stratos"
    - name: GROUP
      value: "Ltd"
    command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
  restartPolicy: Never
```
   <img width="609" height="334" alt="image" src="https://github.com/user-attachments/assets/966c8b14-5dae-4cd5-8f87-7694df709753" />


# Step-2: Apply the manifest and create the pod.
```
kubectl appy -f pod.yaml
```
<img width="605" height="84" alt="image" src="https://github.com/user-attachments/assets/0346e333-1f54-4e91-ae18-304496708339" />

# Step-3: Verify if the environment variables got printed successfully.
```
kubectl describe pod print-envars-greeting
```
<img width="866" height="507" alt="image" src="https://github.com/user-attachments/assets/1e545687-fc7d-42de-8d26-c3352ee8085b" />
<img width="733" height="491" alt="image" src="https://github.com/user-attachments/assets/30f7c855-e04a-4f9f-83f8-d7836eefb177" />
<img width="1259" height="373" alt="image" src="https://github.com/user-attachments/assets/d90d720a-617f-46d8-89d0-34f81d20eb00" />

```
kubectl logs -f print-envars-greeting
```
<img width="571" height="54" alt="image" src="https://github.com/user-attachments/assets/b0eb2f86-0eb8-4ee3-bd23-9871864c9e0f" />




**Explanation:**

- kubectl logs: This command is used to retrieve the standard output and standard error logs from containers within a pod.
- -f: The "follow" flag streams the logs in real-time. Although for a completed pod, it just displays the entire log history.
- print-envars-greeting: The name of the pod whose logs we want to inspect.
- The output Welcome to Stratos Industries confirms that the environment variables were correctly injected into the container, and the echo command successfully concatenated and printed their values to the standard output.

**Conclusion**
This exercise successfully demonstrated the deployment of a Kubernetes pod with custom environment variables and the execution of a specific command. By setting restartPolicy: Never, we ensured that the pod behaved like a completed job, making it ideal for one-off tasks or initialization processes. This foundational understanding of environment variables and pod lifecycle management is critical for building more complex and dynamic applications within a Kubernetes ecosystem.

