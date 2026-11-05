# Day #73 task: Jenkins Scheduled Jobs

The devops team of xFusionCorp Industries is working on to setup centralised logging management system to maintain and analyse server logs easily. Since it will take some time to implement, they wanted to gather some server logs on a regular basis. At least one of the app servers is having issues with the Apache server. The team needs Apache logs so that they can identify and troubleshoot the issues easily if they arise. So they decided to create a Jenkins job to collect logs from the server. Please create/configure a Jenkins job as per details mentioned below:
Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321

1. Create a Jenkins jobs named ***copy-logs***.

2. Configure it to periodically build every 5 minutes to copy the Apache logs (both access_log and error_log) from ***App Server 2 (stapp02)*** from the default logs location to location ***/usr/src/finance*** on the Storage Server.

3. Build the job at least once so that the logs are copied and can be verified.

***Note:***

1. You might need to install some plugins and restart Jenkins. We recommend selecting Restart Jenkins when installation is complete and no jobs are running in the update centre. Refresh the page if the UI gets stuck after a restart.

2. Define the cron expression as required (e.g. */10 * * * * to run every 10 minutes).

3. For scenarios that require web UI changes, take screenshots or record your work (e.g. using loom.com) so you can share it for review if the task is marked incomplete.


- Infrastructure, user and server details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details



## step1: Log in to Jenkins UI and create the job.
<img width="1321" height="362" alt="image" src="https://github.com/user-attachments/assets/5b949485-5381-4a5e-b1f2-2a0792bb3206" />

<img width="1330" height="576" alt="image" src="https://github.com/user-attachments/assets/aa3ca92e-6d35-45af-b433-33d2eeb2183e" />

<img width="1147" height="479" alt="image" src="https://github.com/user-attachments/assets/f85255a9-a54f-4c3e-b7a4-8d8fb93d75b7" />


## step2: Setup configuration in jenkins , so that jenkin communicates with app-server
- Jenkins --> manage jenkins\
   install the ssh relevent plugins (Publish Over SSH, SSH Credentials Plugin, SSH Pipeline Steps, SSH plugin) , then restart jenkings

   <img width="868" height="359" alt="image" src="https://github.com/user-attachments/assets/f692869f-88da-4692-8f9a-3affabb73753" />


- Jenkins --> manage jenkins\
   configure ***credentials*** under ***security*** ( add app-server user name and password, under system, global,  so that jenkin can access that server)

   <img width="1124" height="599" alt="image" src="https://github.com/user-attachments/assets/0bc26f51-02d3-4f73-9e3a-ae96f54bb969" />

 
- Jenkins --> manage jenkins\
   configure ***system*** under ***System Configuration*** , in that add SSH remote hosts (give credentials of server so that jenkin can access that (hostname, port and under credential you will get     server details, which was configured in previou step))

  <img width="1212" height="480" alt="image" src="https://github.com/user-attachments/assets/1f53952a-10c7-401b-9b41-d1eae21ff4de" />

  
  check the connections and save it without fail.

  <img width="1229" height="508" alt="image" src="https://github.com/user-attachments/assets/7eb0a34e-e0fe-49b1-bb34-b4b2f8489016" />

- set Passwordless connection between stapp03 server and the storage-server.
  use sshkeygen

  ```
  ssh-keygen -t rsa
  ```
  just press enter, enter

  <img width="583" height="391" alt="image" src="https://github.com/user-attachments/assets/a2c035ed-76d7-4366-9fb6-546e04a18445" />

- copy those generated keys to storage server
  ```
  ssh-copy-id natasha@ststor01
  ```
- check passwordless connectivity from app-server
  ```
  ssh natasha@ststor01
  ```
  now you will see natasha user prompt, exit from that
  
  <img width="718" height="397" alt="image" src="https://github.com/user-attachments/assets/fc7f3fc1-e43b-406e-8311-4d08017b66d5" />


## step3: just verify mentioned paths, directories and files are present or not, if not create it.

<img width="615" height="270" alt="image" src="https://github.com/user-attachments/assets/14b27a20-66c8-4e76-87db-3924b52a7aa7" />

<img width="737" height="465" alt="image" src="https://github.com/user-attachments/assets/6cc1d4f7-df87-4979-b565-b011920be3e4" />



## step4: Configure the copy-logs job with the 5 minutes schedule and the shell command to copy logs.

configure the job with Triggers(Build periodically) and Build Steps with Execute shell script on remote host using ssh, mentions the commands which you want to perform the actions

<img width="1241" height="439" alt="image" src="https://github.com/user-attachments/assets/3375d90f-5f51-4ceb-a9c8-056a4e257cbe" />

<img width="797" height="282" alt="image" src="https://github.com/user-attachments/assets/a1594c7c-e6df-4328-9a1c-58c1b9a7e28f" />



- I tried with two types both are successfull
  
  1.USING SINGLE COMMAND
    ```
    scp /var/log/httpd/access_log /var/log/httpd/error_log natasha@ststor01:/usr/src/sysops
    ```
     <img width="1030" height="498" alt="image" src="https://github.com/user-attachments/assets/aa4768fd-c74f-43a0-a6e1-44b90c05a7eb" />

   - wait for 5 minutes, the see the console out put.
      <img width="1068" height="445" alt="image" src="https://github.com/user-attachments/assets/b8ce6eed-cc2f-4fca-95d9-d70bc82ef5a0" />

   
   - verify the logs in storage server\
     <img width="645" height="511" alt="image" src="https://github.com/user-attachments/assets/40a49ad0-2008-4985-841b-ca834f799ba0" />

  

  2. USING VARIABLES
     ```
     LOG_DIR="/var/log/httpd" # Confirmed Apache log path on CentOS
     DEST_USER="natasha"
     DEST_HOST="ststor01"
     DEST_PATH="/usr/src/finance/"

     # Executes scp using the previously set up SSH keys
     scp ${LOG_DIR}/access_log ${LOG_DIR}/error_log ${DEST_USER}@${DEST_HOST}:${DEST_PATH}
     ```
    <img width="1181" height="543" alt="image" src="https://github.com/user-attachments/assets/bc7f0f22-0ef6-467d-84df-8e05b504cbbd" />

 - verify the logs in storage server\
   <img width="539" height="221" alt="image" src="https://github.com/user-attachments/assets/12fa9bd3-a05c-40d4-9036-4852f6241c2d" />


 
