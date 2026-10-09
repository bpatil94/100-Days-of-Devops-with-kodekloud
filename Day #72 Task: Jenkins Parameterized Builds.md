# Day #72 task: Jenkins Parameterized Builds

A new DevOps Engineer has joined the team and he will be assigned some Jenkins related tasks. Before that, the team wanted to test a simple parameterized job to understand basic functionality of parameterized builds. He is given a simple parameterized job to build in Jenkins. Please find more details below:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

1. Create a parameterized job which should be named as ***parameterized-job***

2. Add a string parameter named ***Stage;*** its default value should be ***Build***.

3. Add a choice parameter named ***env;*** its choices should be Development, Staging and Production.

4. Configure job to execute a shell command, which should echo both parameter values (you are passing in the job).

5. Build the Jenkins job at least once with choice parameter value Production to make sure it passes.

Note:
1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.
2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.







## Step-1: Log in to your Jenkins Server with the given admin credentials.
<img width="763" height="104" alt="image" src="https://github.com/user-attachments/assets/a9259875-e2b2-4fc9-999d-f349aa09efde" />
<img width="1356" height="619" alt="image" src="https://github.com/user-attachments/assets/5a052aab-be9f-4d54-b229-34a23c42f55f" />

- On successful login, you will be taken to the dashboard landing page. Let’s create a new project/job named as parameterized-job.

<img width="1163" height="414" alt="image" src="https://github.com/user-attachments/assets/1c3b0c39-1c74-46a8-bc5f-4b8c643921cf" />


## Step-2: Create and configure the parameterized-job.

Let’s configure the job to have two parameters:

String Parameter:-
- Name: Stage
- Default Value: Build

<img width="1301" height="603" alt="image" src="https://github.com/user-attachments/assets/0dcb94c3-11f9-45ec-8ce4-78748894fa8d" />

  
Choice Parameter:-
- Name:env
- Choices: Development, Staging and Production
- Description: Target environment for deployment (Optional)
- 
<img width="1025" height="462" alt="image" src="https://github.com/user-attachments/assets/4ad62f2b-41fc-4ff3-b971-ece59ed5e49d" />


## Step-3: Validate by running the parameterized job with the parameters.
- Let’s build the job with the Stage and env parameters ad validate if the parameters are actually being utilized within the job execution.
- Scroll to the Build section
- Click Add build step → Execute shell
- Enter the following shell script
  ```
  echo "Stage: $Stage"
  echo "Environment: $env"
  ```
 <img width="1067" height="542" alt="image" src="https://github.com/user-attachments/assets/21bf974d-0eac-454a-9da2-70742be347be" />

 Optional: Alternatively, you can add more detailed output:
 ```
#!/bin/bash
echo "=================================="
echo "Jenkins Parameterized Build"
echo "=================================="
echo "Stage Parameter: $Stage"
echo "Environment Parameter: $env"
echo "Build Number: $BUILD_NUMBER"
echo "Build Date: $(date)"
echo "=================================="
```

- Click Save at the bottom of the page
- You will be redirected to the job's main page

## Step 4: Build the Job with Parameters
On the job page, click Build with Parameters (instead of the regular "Build Now")
You will see the parameter input form:
Stage: Leave as default Build or enter a different value
env: Select Production from the dropdown
Click Build

<img width="1327" height="623" alt="image" src="https://github.com/user-attachments/assets/01dae43d-ff0c-4068-928e-8a3eaa80e40a" />

<img width="1371" height="531" alt="image" src="https://github.com/user-attachments/assets/98e59978-26b3-4caf-8cf6-5909886ac235" />

## Step 5: Verify Build Success
1. Check the Build History panel on the left
2. Click on the latest build number (e.g., #1)
3. Click Console Output
4. Verify the output shows

- without optional echo commands
<img width="697" height="242" alt="image" src="https://github.com/user-attachments/assets/e1ab88dc-4bb5-4570-b05f-297ef3c109a3" />


- with optional echo commands
<img width="1340" height="638" alt="image" src="https://github.com/user-attachments/assets/0f03a028-c20f-413f-b42b-435425b84944" />




# Understanding Parameterized Builds
What are Parameterized Builds?
Parameterized builds allow you to pass dynamic values to your Jenkins jobs at runtime, making them flexible and reusable. Instead of hardcoding values, you can:

Use different configurations for the same job
Deploy to different environments
Build different branches or versions
Customize build behavior based on input

***Types of Parameters**
<img width="808" height="348" alt="image" src="https://github.com/user-attachments/assets/10a8e9d3-acc1-4a93-8448-c556432f9297" />

***Best Practices***
1. Use Descriptive Names: Make parameter names clear and self-explanatory
2. Set Sensible Defaults: Provide default values for common scenarios
3. Add Descriptions: Help users understand what each parameter does
4. Validate Input: Add validation in your scripts to check parameter values
5. Document Parameters: Include parameter documentation in job description


