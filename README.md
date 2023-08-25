
# Run a Jenkisn Build and Manually Deploy a URL Shortner to Elastic Beanstalk

August 23, 2023

By:  Annie V Lam - Kura Labs

## Purpose
To build and test the URL shortner applicaition using Jenkins and deploy a URL shortner using Elastic Beanstalk.

Previously, an EC2 that already has Jenkins installed was used.  This deployment, requires a new EC2 to be created and Jenkins installed.

### Step #1 Map out the Deployment

[Deployment Flowchart](Images/Deployment_Pipeline.png)

### Step #2 Upload Repository to GitHub

Github is where Jenkins retrieve the files to build and test the application and where Elastic Beanstalk retreives the files to deploy the application.  In order for the EC2, where Jenkins and Elastic Beanstalk is installed, to get access to the repository a token needs to be generated from the GitHub and passed to the EC2s.

### Step #3 and Step #4:  Use Jenkins to Auto Build and Auto Test Application

To use Jenkins in a new EC2, the EC2 needs to be created and all the proper installs to use Jenkins and to read the programing lanuague that the application is written in. In this case, they are Jenkins, Java, Python, and Jenkins additional plugin "Pipeline Utility Steps".

Log into Jenkins create a build "Deployment02" for the URL Shortner application from GitHub Repository https://github.com/LamAnnieV/Run_Jenkins_Build_and_Deploy_to_EB_Deployment02.git and run the build

### INSTRUCTIONS for INSTALLS to an Ubuntu OS in AWS

### BASH commands to install JAVA pacakage(s):
-   $sudo apt update   
-   $sudo apt-get install fontconfig openjdk-17-jre

### BASH commands to install JENKINS pacakage(s):  
Official Instaltion Instructions can be found here: https://pkg.jenkins.io/debian/
-   $curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
-   $echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] tps://pkg.jenkins.io/debian binary/ | sudo tee tc/apt/sources.list.d/jenkins.list > /dev/null
-   $sudo apt update
-   $sudo apt-get install jenkins
-   $sudo systemctl start jenkins
-   $sudo systemctl status jenkins

### BASH commands to install PYTHON pacakage(s):
-   $sudo apt update
-   $sudo apt install python3.10-venv

### Results

****The Build Stage was Successful, but not the Test Stage****

![Jenkins Unsuccessful Build](Images/Jenkins_Unsuccessful.png)

**Initial Analysis of Failure:**

-     The two errors are related to a file not found and a command not found.  The command is py.test, and py stands for python.  There might be an issue with the python install.

![Jenkins Output for Unsuccessful Build](Images/Jenkins_Output_Issues.png)

**Analysis:**
-     From the output file, pip and py.test commands are not found.  Both commands are related to Python

**Resolution:**

-     Reinstall Python in the Server, using the BASH commands to install PYTHON pacakage(s)

****Successful Build:****

![Jenkins Successful Build](Images/Jenkins_Success.png)


### Step #5:  Download URL Shortner Application Repository from GitHub to be uploaded to the Production Server to be Deployed

Download Repository, Unzip files and re-zip files

### Step #6:  Deploy Application on AWS ELASTIC BEANSTALK

**Create EC2 Role**

-     AWS/IAM/Roles/Create Role/Select:  AWS Service/[Use Case] Select:  EC2/Next
-     [Permissions Policies] Select:   "AWSElasticBeanstalkWebTier" & “AWSElasticBeanstalkWorkerTier”/Next
-     Role Name:  Elastic-EC2/Create Role

**Create EBS Role**

-     AWS/IAM/Roles/Create Role/Select:  AWS Service/[Use Case] Use Cases for other AWS services:  Elastic Beanstalk/Select:  Elastic Beanstalk - Customizable/Next
-     Next
-     Role Name:  aws-elasticbeanstalk-service-role/Create Role

**Deploy URL Shortner application in Elastic Beanstalk**

-     AWS/Elastic Beanstalk/Environments/Create Environment/Application Name:/[Platform-4] Platform:  Python/Platform Branch:  Python 3.9 running on 64bit Amazon Linux 2023/Select:  Upload Your code/Version Label:  v#/Select: Local File/Choose File:  {files that was downloaded from GitHub, Unzipped, then rezipped}/Next
-     [EC2 instance profile] Select:  Elastic-EC2/Next
-     [Virtual Private Cloud] Select:  default VPC/[Instance Subnets] Select:  us-east-1a/Next
-     [Instances] Root Volume Type:  General Purpose (SSD)/Size:  10/[Capacity] Instance Types:  Deselect all & Select t2.micro/Next
-     Next
-     Submit
**Successful**

![URL Shortener Successfully Deployed](Images/EBS_Results.png)
 
### Step #7:  Launch Website

![URL Shortener Successfully Deployed](Images/URL_Shortner.png)
            
### Optimization
-     Scripts to Install Applications in the Virtual Machine
-     Scripts to dectect updates in Hithub and fully automate the build and test stage in Jenkins
-     Auto sent files to EBS server to deploy when the Build in Jenkins is Successful 


  
