# Day #71 task: Configure Jenkins Job for Package Installation

Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:

1. Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username **admin** and password **Adm!n321.**

2. Create a new Jenkins job named **install-packages** and configure it with the following specifications:

Add a string parameter named **PACKAGE**.

Configure the job to install a package specified in the $PACKAGE parameter on the storage server (Stratos Datacenter).

Build the job at least once (e.g. with parameter PACKAGE=vim-enhanced) so the package is installed on the Storage server and can be verified.


- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details
**Note:**

1. Ensure to install any required plugins and restart the Jenkins service if necessary. Opt for Restart Jenkins when installation is complete and no jobs are running on the plugin installation/update page. Refresh the UI page if needed after restarting the service.

2. Verify that the Jenkins job runs successfully on repeated executions to ensure reliability.

3. Capture screenshots of your configuration for documentation and review purposes. Alternatively, use screen recording software like loom.com for comprehensive documentation and sharing.




## Step-1: Log in to Jenkins UI and create the job.
Let’s create a new job (freestyle-project) and name it ‘install package’.

<img width="770" height="600" alt="image" src="https://github.com/user-attachments/assets/4ae30d42-6db4-4730-b7fd-914a4c00b0a4" />

<img width="1362" height="716" alt="image" src="https://github.com/user-attachments/assets/76266f65-dfd6-4b26-86c0-21833094c3f5" />


## Step-2: Configure the ‘install-package’ job.
Let’s configure the job. As the problem statement mentioned, we need to add a string parameter named ‘PACKAGE’. You can add the description and default values, if you would like to.

 <img width="1361" height="714" alt="image" src="https://github.com/user-attachments/assets/7b4b3280-ae66-4aba-88b8-e26c19f15510" />


Now, before we add the command to install the package, let’s check out the flavor of the os image of the storage server so, we can use the right library to install the package.

<img width="668" height="508" alt="image" src="https://github.com/user-attachments/assets/01949ad7-57bb-4e7c-9fac-ea060fad08de" />

Since, the storage server uses CentOS stream 9, we need to make use of the ‘dnf’ package to install the packages in the storage server.

So, let’s add the command to install the package in the build step.

***ssh natasha@ststor01 sudo dnf makecache && sudo dnf install -y $PACKAGE***

<img width="1360" height="666" alt="image" src="https://github.com/user-attachments/assets/4284e63f-6b38-4ae8-ad6a-a2b6c048115b" />

But, we will face one issue, if we try to run this job. Since the jenkins server is not aware of the password or the key to ssh into the storage machine, authentication will fail. So, let’s set up passwordless connection to the storage server on the jenkins server.

## Step-3: Setup Passwordless connection to the storage server from the jenkins server.

<img width="686" height="494" alt="image" src="https://github.com/user-attachments/assets/2d9d56c9-a953-4c94-a9d9-1fe9834cd7b4" />

### Let's set up passwordless connection between the storage server and the jenkins server.
### Create the Public key.

<img width="671" height="397" alt="image" src="https://github.com/user-attachments/assets/56e1ce77-0397-4962-a6e9-8e231740f8df" />

### Copy the same id to the storage server.

<img width="728" height="360" alt="image" src="https://github.com/user-attachments/assets/f3db4d5f-ccd6-4379-b565-ee260af4a1a0" />

Now that passwordless connection is setup, let’s run the build.



## Step-4: Run the build and validate if package is installed successfully.

- Before running the job, check if package exists:
  <img width="651" height="152" alt="image" src="https://github.com/user-attachments/assets/ce798348-7187-4ec5-98fe-b64d47b1a0e9" />

  Enter the package that needs to be installed (as given in the problem statement: vim-enahanced).

  <img width="1189" height="509" alt="image" src="https://github.com/user-attachments/assets/8799f591-5c3e-4fb9-ba14-f7d4efebc524" />



<img width="1350" height="609" alt="image" src="https://github.com/user-attachments/assets/47f5a7b3-1b31-4cfb-9b20-1f4a3a194d5c" />

<img width="1348" height="508" alt="image" src="https://github.com/user-attachments/assets/98311530-af41-4ba5-a9b2-7766861caf5f" />

<img width="1226" height="513" alt="image" src="https://github.com/user-attachments/assets/117af612-0e5e-4505-948d-d77dd5b1027a" />



