# Day #68 task: Set Up Jenkins Server
The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:

1. Install Jenkins on the jenkins server using the apt utility only, and start it using the service command.

If you face a timeout issue while starting the Jenkins service, first check the service status with service jenkins status
Then review the logs in /var/log/jenkins/jenkins.log to identify the cause.


2. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Ravi and email should be ravi@jenkins.stratos.xfusioncorp.com.


Note:

1. To access the jenkins server, connect from the jump host using the root user with the password S3curePass.

2. After Jenkins server installation, click the Jenkins button on the top bar to access the Jenkins UI and follow on-screen instructions to create an admin user.




***What is Jenkins?***
Jenkins is an open-source tool used to automate software development tasks like building, testing, and deploying applications.

In simple terms, Jenkins helps developers save time and avoid manual work by running these steps automatically whenever code is updated.

***Why use Jenkins?***
It automates repetitive tasks and helps find errors early (especially in a release cycle for corporate workflows.)
Speeds up software delivery.
This is how a typical Jenkins workflow looks like:

### Code push → Jenkins builds → runs tests → deploys.


# Step-1: Log in to your Jenkins Server as the root user and install Jenkins.

- Log in to the Jenkins server as the root user using the given password.

```
ssh root@jenkins
```
<img width="634" height="326" alt="image" src="https://github.com/user-attachments/assets/a42e929a-3904-4a27-90e8-40fb2a950c07" />


Now that we are in the jenkins server, let’s work on installing Jenkins! You can follow through their official documentation for reference: https://www.jenkins.io/doc/book/installing/linux/

As pre-requisite (as mentioned in the doc), we need to install java and it’s dependencies as Jenkins need Java to run.


-  Check if Java is already installed.
```
java -version
```
<img width="485" height="79" alt="image" src="https://github.com/user-attachments/assets/afd40478-ce4d-4926-bdd6-48a8a551db53" />

- Since it's not installed, let's install it.

```
sudo apt update
```
```
sudo apt install fontconfig openjdk-21-jre
```
<img width="686" height="394" alt="image" src="https://github.com/user-attachments/assets/b5f1d562-f0fd-4151-99e7-d17957675519" />


```
java -version
```
<img width="660" height="92" alt="image" src="https://github.com/user-attachments/assets/65ad499a-c030-4ed7-99b1-a86c97faf596" />


```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```


```
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```
<img width="689" height="326" alt="image" src="https://github.com/user-attachments/assets/585847d0-e038-46cc-ab34-c8774d1034b2" />

```
sudo apt update
```
<img width="635" height="248" alt="image" src="https://github.com/user-attachments/assets/e35f7f66-ce84-49dd-b09a-02f09e0e5e81" />

```
sudo apt install jenkins
```
<img width="727" height="359" alt="image" src="https://github.com/user-attachments/assets/4c0d0cda-c6fa-44c4-a1ec-b5170d659c8a" />


- Let’s check the status of Jenkins using the servicecommand as mentioned in the problem statement and start it, if not started.

```
service jenkins status
```
<img width="414" height="55" alt="image" src="https://github.com/user-attachments/assets/869d6aff-df7c-4888-be17-7dcdb5770195" />

```
service jenkins start
```
<img width="396" height="68" alt="image" src="https://github.com/user-attachments/assets/185c7eac-f3fb-450d-a172-7336e94c2062" />


```
service jenkins status
```
<img width="475" height="52" alt="image" src="https://github.com/user-attachments/assets/8737f8fe-fc44-4a9a-824e-f969ecf20549" />


- Now that the Jenkins service is actually running, you should be able to access the app and create the user as mentioned in the problem statement.

# Step-2: Access the Jenkins app and create the user.

- Click on the Jenkins button as highlighted in the screenshot below.

<img width="732" height="561" alt="image" src="https://github.com/user-attachments/assets/6ada4ae6-38dd-4d15-bad9-d85903a8b723" />


- On clicking that button, you should be taken to a different tab that looks similar to the below screenshot.

<img width="1359" height="709" alt="image" src="https://github.com/user-attachments/assets/64253efe-422c-4364-b22a-fe70dc739fe2" />


- Let’s follow the instructions as shown and complete the user creation. In your terminal, print the content of the file given in the screenshot and paste it in here.
- Copy the output of this file and paste it in the Administrator password.
```
cat /var/lib/jenkins/secrets/initialAdminPassword
```

<img width="675" height="86" alt="image" src="https://github.com/user-attachments/assets/644f9999-2a7f-453c-b7f2-cdb3dc0a54de" />

- On entering the correct password, you should be taken to the next page, where you should install the recommended.

<img width="1004" height="585" alt="image" src="https://github.com/user-attachments/assets/4da08da5-c6ca-4146-8352-ee88e2c3df48" />



-On clicking it, it will start installing all the recommended plugins.


<img width="1019" height="589" alt="image" src="https://github.com/user-attachments/assets/d6877ea5-91a7-43f7-bf4d-b6fa19126442" />

<img width="1012" height="587" alt="image" src="https://github.com/user-attachments/assets/5baedeb8-c883-4f08-bd83-98f84563f774" />


Now enter the first admin user details as given in the problem statement:
- Admin Username: theadmin
- Admin Password: Adm!n321
- Admin Name: Ravi
- Admin Email: ravi@jenkins.stratos.xfusioncorp.com

<img width="1016" height="571" alt="image" src="https://github.com/user-attachments/assets/40233ab8-d7b0-4535-b2b0-d5f749dbb750" />



- Ensure that the instance is the same as where your jenkins server is running. ( It’ll be auto-populated.)

<img width="1072" height="431" alt="image" src="https://github.com/user-attachments/assets/22e650e3-a8cf-4f7c-9c43-fa2ecc4ba542" />


- And with that, you should see this as your dashboard page!

<img width="1355" height="719" alt="image" src="https://github.com/user-attachments/assets/ce3f0f77-331c-4029-8dbb-9cd457232e89" />



- Just to validate, you can logout and login as the created admin user. And hey! Feel free to explore Jenkins using your current setup!

<img width="1361" height="602" alt="image" src="https://github.com/user-attachments/assets/86a28818-e8c8-4d45-9c16-99ccdaeb8df7" />


- able to log in

<img width="1365" height="660" alt="image" src="https://github.com/user-attachments/assets/9ba8e5bb-3dde-4a68-8356-e56a09af0706" />




