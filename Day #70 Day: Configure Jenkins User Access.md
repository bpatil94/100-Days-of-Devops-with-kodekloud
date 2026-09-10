# Day #70 task: Configure Jenkins User Access
The Nautilus team is integrating Jenkins into their CI/CD pipelines. After setting up a new Jenkins server, they're now configuring user access for the development team, Follow these steps:

1. Click on the Jenkins button on the top bar to access the Jenkins UI. Login with username admin and password Adm!n321.

2. Create a jenkins user named rose with the password LQfKeWWxWD. Their full name should match Rose.

3. Utilize the Project-based Matrix Authorization Strategy to assign overall read permission to the rose user.

4. Remove all permissions for Anonymous users (if any) ensuring that the admin user retains overall Administer permissions.

5. For the existing job, grant rose user only read permissions, disregarding other permissions such as Agent, SCM etc.


Note:

1. You may need to install plugins and restart Jenkins service. After plugins installation, select Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page.


2. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding. Avoid clicking Finish immediately after restarting the service.


3. Capture screenshots of your configuration for review purposes. Consider using screen recording software like loom.com for documentation and sharing.




# Step-1: Log in to your Jenkins Server with the given admin credentials.
<img width="773" height="205" alt="image" src="https://github.com/user-attachments/assets/b387f32e-d4ea-4d7c-aa31-c6649bdcf61f" />

<img width="1354" height="714" alt="image" src="https://github.com/user-attachments/assets/cfa50511-2964-4a92-b68c-50c627188849" />


- On successful login, you will be taken to the dashboard landing page.

<img width="1363" height="453" alt="image" src="https://github.com/user-attachments/assets/ee7c7ca3-6fa3-4362-a4bb-761e9adcb24a" />

  
# Step-2: Create the new user.
1. Navigate to: Manage Jenkins → Users → Create User
2. Fill in the details:
Username:rose
Full Name: Rose
Password: LQfKeWWxWD

<img width="1349" height="599" alt="image" src="https://github.com/user-attachments/assets/f3405885-8f53-4caa-916a-0439c2330c72" />



# Step-3: Assign the required permissions for the newly created user.

As mentioned in the problem statement, we need to utilize the Project-based Matrix Authorization Strategyto assign the required permission. So, let’s install the plugins needed for this. You will need to install the ‘Matrix Authorization Strategy’ plugin for this.
1. Navigate to: Manage Jenkins → Plugins → Available Plugins
2. Search for: Matrix Authorization Strategy
3. Click Install without restart.
4. Once done, select: Restart Jenkins when installation is complete and no jobs are running.
5. Wait until the Jenkins login page reloads.

<img width="1356" height="439" alt="image" src="https://github.com/user-attachments/assets/9bbd22b0-f92c-415b-bb7a-3f87e994dcec" />


- Now, let’s assign the permissions for the newly created user.
  1. Go to: Manage Jenkins → Security → Configure Global Security
  2. Under Authorization, select: ✅ Project-based Matrix Authorization Strategy
  In the permission table, assign as follows:

- Add the rose user and select the overall read permission for this user.

<img width="1260" height="488" alt="image" src="https://github.com/user-attachments/assets/4e39208d-e4c5-468e-b58c-b063e1fdb7b1" />

<img width="1209" height="452" alt="image" src="https://github.com/user-attachments/assets/14a0b71b-9306-4447-9313-16c56f72ffe4" />

- save the things


- Now, let’s also define the permission at the individual job level for the user.

  1. Open any existing Jenkins job.
  2. Click Configure.
  3. Scroll down to Build Permissions or Enable project-based security.
  4. Add user jim and grant only Read access.
  5. Click Save.

<img width="1322" height="613" alt="image" src="https://github.com/user-attachments/assets/4fbe9118-0086-4c5c-978a-c47e58546553" />


# Step-4: Validate if the permissions are implemented.
Now, login to Jenkins using the newly created user credentials and validate the read permission implementation for the user. In the landing dashboard page, you shouldn’t see any of the settings that we see as an admin. At the individual job level, you should be able to do anything, except just read the job.

<img width="1342" height="557" alt="image" src="https://github.com/user-attachments/assets/a1631866-b725-42bb-a69d-72913570b5ee" />


- rose used should be able to read the build

  <img width="1353" height="578" alt="image" src="https://github.com/user-attachments/assets/19c65857-cb66-4d4d-ad72-6ffafb2015a7" />

