# Day #69 task: Install Jenkins Plugins

The Nautilus DevOps team has recently setup a Jenkins server, which they want to use for some CI/CD jobs. Before that they want to install some plugins which will be used in most of the jobs. Please find below more details about the task

1. Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

2. Once logged in, install the Git and GitLab plugins. You may need to restart Jenkins to complete the plugin installation; if required, opt to Restart Jenkins when installation is complete and no jobs are running on the plugin installation/update page (Update Centre).

**Note:**

1. After restarting Jenkins, wait for the login page to reappear before proceeding.

2. For tasks involving web UI changes, capture screenshots to share for review or consider using screen recording software like loom.com for documentation and sharing.



# Step-1: Log in to your Jenkins Server with the given credentials.

<img width="724" height="522" alt="image" src="https://github.com/user-attachments/assets/3f304b7c-c99d-40a2-bffc-b8164b3e2e89" />


Click on that button as seen in the screenshot and you’ll be taken to the Jenkins login page. Do login with the credentials given:
- Username: admin
- Password: Adm!n321

<img width="1363" height="717" alt="image" src="https://github.com/user-attachments/assets/1152e5b1-f165-477d-b982-443132acef37" />


Yes, you should be able to see the dashboard, something similar to the below screenshot.

<img width="1354" height="714" alt="image" src="https://github.com/user-attachments/assets/ad8ed7b1-bfc8-4907-a65e-1425472dadfb" />

# Step-2: Install the required plugins and restart Jenkins.

- Click on the settings icon (the gear icon as highlighted in the screenshot) and click on the plugins (as highlighted in the screenshot).

<img width="1347" height="591" alt="image" src="https://github.com/user-attachments/assets/7543cc35-e326-4667-bcc7-d118ab7152ab" />


- Look for Git and GitLabplugins and install them.

<img width="1365" height="711" alt="image" src="https://github.com/user-attachments/assets/6675d44e-f44c-4786-9923-eaff3ead0728" />


Click on “Restart Jenkins when installation is complete and no jobs are running” checkbox, so that it restarts jenkins after the plugins are downloaded and installed.

<img width="1359" height="553" alt="image" src="https://github.com/user-attachments/assets/95cc3b7a-e951-40ee-a203-466e75a78161" />

# Step-3: Verify if the plugins are successfully installed.

<img width="1358" height="622" alt="image" src="https://github.com/user-attachments/assets/e79de1d0-cf76-40c8-b5f8-cb7d253525fb" />

