# DevOps Practical

## Things to Check Before Proceeding

### Check That Git Is Installed in the System
1. **Verify Installation**: 
   ```bash
   git --version
   ```
   If not installed, visit [Git SCM](https://git-scm.com/) and download.

2. **Verify Java Installation**:
   ```bash
   java --version
   ```
   - Java version should be 8, 11, or 21.
   - If not installed, download and install Java, and set the path in environment variables:
     - `JAVA_HOME`
     - Paste the path of the `bin` folder of Java 8, 11, or 21.

---

## Step 1: Setup Git Repository

1. **Create a Folder**: Save the Java code in the folder.
2. **Initialize Git**: Open Git Bash in the folder and run:
   ```bash
   git init
   ```
3. **Stage Java Files**:
   ```bash
   git add .
   ```
4. **Commit Changes**:
   ```bash
   git commit -m "<any message>"
   ```
5. **Link to Remote Repository**:
   - Create a repository on [GitHub](https://www.github.com).
   - Copy the repository link and run:
     ```bash
     git remote add origin <paste link here>
     ```
6. **Push to GitHub**:
   ```bash
   git push -u origin master
   ```
   Or:
   ```bash
   git push -u origin main
   ```
**If any error occur use this commands:**
   - echo "HELLO">README.md
   - git add .
   - git commit -m "<your message>"
   - git push -u origin main or master as per case
---

## Step 2: Download and Setup Jenkins

### For macOS:
1. Install Jenkins:
   ```bash
   brew install jenkins-lts
   ```
2. Start Jenkins:
   ```bash
   brew services start jenkins-lts
   ```
3. Access Jenkins:
   [http://localhost:8080](http://localhost:8080)
4. Copy Password:
   ```bash
   cat /usr/local/var/jenkins/home/secrets/initialAdminPassword
   ```

### For Windows:
1. Download Jenkins:
   - Visit [Jenkins Download Page](https://www.jenkins.io/download).
   - Choose Windows and download the `.msi` installer.
2. Install Jenkins:
   - Follow the installation wizard.
   - Select "Install Suggested Plugins" when prompted.
   - Provide the path to your Java JDK.
   - Set the installation folder.
   - Configure the service port (default: 8080).
3. Retrieve Initial Password:
   - Navigate to:
     ```
     C:\Program Files\Jenkins\.jenkins\secrets
     ```
   - Open the `initialAdminPassword` file and copy the password.

---

## Step 3: Create a Jenkins Job

1. **Open Jenkins**:
   [http://localhost:8080](http://localhost:8080)
2. **Create a New Job**:
   - Click on "New Item" in the dashboard.
   - Enter a job name.
   - Select "Freestyle Project" and click OK.
3. **Configure the Job**:
   - Go to the "Source Code Management" section.
   - Select "Git".
   - Enter the GitHub repository URL.
   - Choose the branch (main or master).
4. **Build Trigger**:
   - Check "Poll SCM" to let Jenkins check for changes.
   - In the schedule field, enter:
     ```
     H/5 * * * *
     ```
5. **Build Steps**:
   - In the "Build" section, add a step:
     - **For macOS**: Select "Execute Shell".
     - **For Windows**: Select "Execute Windows batch command".
   - Enter commands to compile and run Java code:
     ```bash
     javac <program_name>.java
     java <program_name>
     ```
6. **Save and Build the Job**:
   - Click "Save".
   - Click "Build Now".
7. **Verify the Build Output**:
   - Go to "Build History".
   - Click on the build number and view the console output.

---

## Step 4: Automate the Build

1. **Configure Webhook in GitHub**:
   - Go to the repository settings.
   - Navigate to "Webhooks" and click "Add webhook".
   - Set the Payload URL to:
     ```
     http://localhost:8080/github-webhook/
     ```
   - Select "application/json" as content type.
   - Choose "Just the push event".

2. **Install GitHub Integration Plugin in Jenkins**:
   - Go to "Manage Jenkins > Manage Plugins".
   - Search for "GitHub Integration Plugin" and install it.

3. **Update Jenkins Job Configuration**:
   - Enable the "GitHub hook trigger for GITScm polling" option in the job configuration.

4. **Test Automation**:
   - Push changes to the GitHub repository.
   - Verify the new build is triggered in Jenkins.
   - Check the console output for successful build and execution.

---

## Step 5: Deploy Application Locally

1. **Create a Deployment Script**:
   - For Windows (`deploy.bat`):
     ```batch
     @echo off
     echo Starting the HelloWorld application...
     java <program name>
     pause
     ```
   - For macOS (`deploy.sh`):
     ```bash
     #!/bin/bash
     echo "Starting the HelloWorld application..."
     java <program name>
     ```

2. **Test the Script**:
   - Run the script locally.
   - Verify the application runs as expected.

3. **Add the Script to Jenkins**:
   - In Jenkins, navigate to the job configuration.
   - Add a new build step to execute the script:
     - **Windows**:
       ```bash
       `deploy.bat`
       ```
     - **macOS**:
       ```bash
       chmod +x deploy.sh
       ./deploy.sh
       ```

---

## Step 6: Test the Pipeline (CI/CD)

1. **Make Changes to the Code**:
   - Edit the Java file.
   - Save the changes.

2. **Push Changes to the Repository**:
   ```bash
   git add <filename>
   git commit -m "Updated message"
   git push origin master
   ```
   or
   ```bash
   git add <filename>
   git commit -m "Updated message"
   git push origin main
   ```
   

4. **Observe Jenkins Automation**:
   - Verify a new build is triggered automatically.
   - Check the console output for successful build and deployment.

5. **Verify the Application**:
   - Test the application locally.
   - For web services, test the local URL.

6. **Troubleshoot Issues**:
   - Check Jenkins console output for errors.
   - Verify environment setup and configurations.

---
