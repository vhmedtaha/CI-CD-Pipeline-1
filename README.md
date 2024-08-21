# Jenkins CI/CD Pipeline with SonarQube and Nexus on AWS EC2



![Screenshot from 2024-08-21 16-03-03](https://github.com/user-attachments/assets/d57bf7ec-3496-4bb1-9875-9fc625820a0a)


This project demonstrates how to set up a CI/CD pipeline using Jenkins, SonarQube, and Nexus on AWS EC2 instances. The pipeline analyzes code using SonarQube and pushes the built artifact to the Nexus repository.

## Project Setup

### Step 1: Create EC2 Instances

1. **Jenkins Instance**
   - **Instance Type:** t2.micro (or your preferred instance type)
   - **Security Group Rules:**
     - SSH (Port 22) - Source: Your IP
     - HTTP (Port 8080) - Source: Anywhere

2. **SonarQube Instance**
   - **Instance Type:** t2.micro (or your preferred instance type)
   - **Security Group Rules:**
     - SSH (Port 22) - Source: Your IP
     - HTTP (Port 80) - Source: Anywhere

3. **Nexus Instance**
   - **Instance Type:** t2.micro (or your preferred instance type)
   - **Security Group Rules:**
     - SSH (Port 22) - Source: Your IP
     - HTTP (Port 8081) - Source: Anywhere

### Step 2: Configure Security Groups

Ensure that the security group rules for each EC2 instance are correctly configured to allow communication between the instances and external access where necessary.

### Step 3: Install Jenkins Plugins

Once Jenkins is set up, install the following plugins:

- **Nexus Artifact Uploader**
- **SonarQube Scanner**
- **Timestamp**
- **Pipeline Maven Integration**
- **Pipeline Utility Steps**

### Step 4: Configure Jenkins

1. **Install Required Tools:**
   - Ensure that Java, Maven,SonarQube Scanner tool and Git are installed on the Jenkins server.
   - Configure Jenkins to use the installed tools.

2. **Configure Jenkins to Integrate with SonarQube:**
   - Install the SonarQube Scanner plugin.
   - Set up SonarQube servers in Jenkins under `Manage Jenkins > Configure System > SonarQube Servers`.
   - ![Screenshot from 2024-08-21 16-18-04](https://github.com/user-attachments/assets/789bb72b-aa27-478c-936f-19b357896dd4)
   - put the puplic ip for sonar instance
   - make the authentication from sonarqube server
   - ![Screenshot from 2024-08-21 16-17-48](https://github.com/user-attachments/assets/1a4de05a-a67c-4a5f-8898-9363eddc38f0)



3. **Configure Jenkins to Integrate with Nexus:**
   - Install the Nexus Artifact Uploader plugin.
   - Configure Nexus credentials and repository details in Jenkins.
   - create repository in nexus server
   - ![Screenshot from 2024-08-21 16-34-11](https://github.com/user-attachments/assets/b0419e17-5af3-46fb-bbfe-24cc34054cb7)


### Step 5: Create Jenkins Pipeline

1. **Create a Jenkins Pipeline:**
   - Use the Jenkinsfile to define the pipeline stages:
     1. **fetch Code:** Pull the code from a version control system (e.g., Git).
     2. **Build:** Compile the code using Maven.
     3. **test:** Test the code using Maven.
     4. **checkstyle:** Analyze the code with Checkstyle.
     5. **Code Analysis:** Analyze the code with SonarQube.
     6. **Quality gates:** put quality gates. 
     7. **Artifact Upload:** Push the artifact to Nexus. 

## check the pipeline status 
![result](https://github.com/user-attachments/assets/0dcd5832-a6d2-4360-955f-513a36cb5460)
<br>
<br>
![good](https://github.com/user-attachments/assets/33e6d154-bd73-4b2c-9e35-2965c5e29bc0)
<br>
<br>
![artifact](https://github.com/user-attachments/assets/b346f26e-2691-4166-8a14-624cb3edf31d)




## Conclusion

This setup allows you to automate the build, code analysis, and artifact storage processes using Jenkins, SonarQube, and Nexus. The EC2 instances ensure that each service is isolated and secure.

## Future Enhancements

- build Docker image that contain the Artifact and push the image to AWS 'ECR'  ==> The next project 
- Automate the creation of EC2 instances and the configuration process using Infrastructure as Code (IaC) tools like Terraform or AWS CloudFormation.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
