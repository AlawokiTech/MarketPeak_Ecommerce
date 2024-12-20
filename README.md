# Introduction to Cloud Computing

## Table of Contents
1. [Introduction](#introduction)
2. [Git Version Control Setup](#1-git-version-control-setup)
   - [Initialize Git Repository](#11-initialize-git-repository)
   - [Obtain and Prepare the E-Commerce Website Template](#12-obtain-and-prepare-the-e-commerce-website-template)
   - [Stage and Commit the Template to Git](#13-stage-and-commit-the-template-to-git)
   - [Push the Code to Your GitHub Repository](#14-push-the-code-to-your-github-repository)
3. [AWS Deployment](#2-aws-deployment)
   - [Set Up an AWS EC2 Instance](#21-set-up-an-aws-ec2-instance)
   - [Clone the Repository on the Linux Server](#22-clone-the-repository-on-the-linux-server)
   - [Install a Web Server on Linux Server (EC2)](#23-install-a-web-server-on-linux-server-ec2)
   - [Configure httpd for Website](#24-configure-httpd-for-website)
   - [Access Website from Browser](#25-access-website-from-browser)
4. [Continuous Integration and Deployment Workflow](#3-continuous-integration-and-deployment-workflow)
5. [Troubleshooting](#4-troubleshooting)

## Introduction

In this project, I have been assigned to develop an e-commerce website for a new online marketplace named **MarketPeak**. This platform will feature product listings, a shopping cart, and user authentication. The objective is to utilize Git for version control, develop the platform in a Linux environment, and deploy it on an AWS EC2 instance. I have been provided with a suitable website template from [Tooplate's Waso Strategy template](https://www.tooplate.com/view/2130-waso-strategy).

## 1. Git Version Control Setup

### 1.1. Initialize Git Repository
- Create a directory named `MarketPeak_Ecommerce`.
- Initialize a Git repository to manage version control.

  ```sh
  mkdir MarketPeak_Ecommerce
  cd MarketPeak_Ecommerce
  git init
  ```

![Git repo.](./img/01.Init_git_repo.png)

#### 1.2. Obtain and Prepare the E-Commerce Website Template
- Download a suitable e-commerce website template from [link to choose from](https://www.tooplate.com/view/2130-waso-strategy).
![template sample.](./img/02.website_template.png.png)

- Extract the downloaded template into your project directory, `MarketPeak_Ecommerce`.
![template extract.](./img/03.template.png)

#### 1.3. Stage and Commit the Template to Git
- Add your website files to the Git repository.
- Set your Git global configuration with your username and email.
- Commit your changes with a clear, descriptive message.
  ```sh
  git add .
  git config --global user.name "YourUsername"
  git config --global user.email "youremail@example.com"
  git commit -m "Initial commit with basic e-commerce site structure"
  ```

    ![stage & commit template](./img/04.stage_and_commit_template_git.png)

#### 1.4. Push the code to your Github repository
- Create a remote repository on GitHub named **MarketPeak_Ecommerce.**

    ![create remote repo](./img/05.github_repo.png)

- Link your local repository to GitHub and push your code.
  ```sh
  git remote add origin https://github.com/AlawokiTech/MarketPeak_Ecommerce.git
  git push -u origin main/master
  ```

    ![link remote repo to local repo](./img/06.local_repo_to_github.png)

## 2. AWS Deployment

### 2.1. Set Up an AWS EC2 Instance
- Log in to the AWS Management Console
![AWS Mgmt Console](./img/07.mgmt_console.png)

- Launch on EC2 instance using Amazon Linus AMI
![Launch EC2](./img/08.MarketPeak_EC2.png)

- Connect to the instance using SSH.
![Connect to EC2 using SSH](./img/09_connect_using_ssh.png)

### 2.2. Clone the repository on the Linux Server
Before deploying the e-commerce platform, we need to clone the GitHub repository to the AWS EC2 instance that has been created. This process involves authenticating with GitHub and choosing between two primary methods of cloning a repository: `SSH` and `HTTPS`.
- Navigate to the repository in Github console, select code, select SSH under clone section and copy the SSH link as highlight in the image below

    ![GitHub Console](./img/10.github_console.png)

- Clone the remote repository on the Linux Server
    - Below tasks need to be completed before cloning remote repository on the Linux Server
    - Generate SSH keypair using ssh-keygen 
![Keypair gen](./img/11.ssh_keypair_gen.png)
    - To display and copy our public key
![rsa.public key display](./img/12.clone_git_repo_ec2.png)
    - add the copied SSH public key to our GitHub account
        - Go to GitHub and log in to your account.
        - In the upper-right corner of any page, click your profile photo, then click Settings.
        ![SSH to GitHub1](./img/13.1.png)
        ![SSH to GitHub2](./img/13.2.png)
        - In the user settings sidebar, click SSH and GPG keys.
        ![SSH to GitHub3](./img/13.3.png)
        - Click New SSH key or Add SSH key.
        - In the "Title" field, add a descriptive label for the new key.
        - Paste your key into the "Key" field.
        - Click Add SSH key.
        ![SSH to GitHub4](./img/13.4.png)
        - If prompted, confirm your GitHub password.
        - There is need to install git on the Linux Server if not installed
        ![git insatllation](./img/14.install_git.png)
         - Clone our remote repository on the Linux Server

        ```sh
        git clone https://github.com/AlawokiTech/MarketPeak_Ecommerce.git
        ```

### 2.3. Install a Web Server on Linux Server (EC2)
To install a web server on the EC2 instance, we will deploy Apache HTTP Server (httpd). Apache is a widely used web server that serves HTML files and content over the internet. Installing it will enable us to host the MarketPeak e-commerce site.

```sh
    - sudo yum update -y
    - sudo yum install httpd -y
    - sudo systemctl start httpd
    - sudo systemctl enable httpd
```
    - sudo yum update -y: This updates the Linux server.
    - sudo yum install httpd -y: This installs httpd (Apache).
    - sudo systemctl start httpd: This starts the web server.
    - sudo systemctl enable httpd: This ensures the web server automatically starts on server boot.


### 2.4. Configure httpd for Website
- Remove any default files and copy your project files to the web server directory.

    To be able to serve the website from the EC2 instance, we need to configure the httpd to point to the directory on the linux server where the website code files are stored.

    It is usually stored in `var/www/html` directory. This directory is a standard directory structure on Linux systems that host web content, particularly for Apache HTTP Server. Thus directory is automatically created when Apache is installed on the system.
```sh
    - sudo rm -rf /var/www/html/*
    - sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/
    - sudo systemctl reload httpd
```

    - sudo rm -rf /var/www/html/*: This forcefully removes all files and directories within the /var/www/html/ directory.
    - sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/: This copies all files and directories from the ~/MarketPeak_Ecommerce/ directory to the /var/www/html/ directory.
    - sudo systemctl reload httpd: This reloads the Apache HTTP Server (httpd) configuration without restarting the service.

![Configure httpd](./img/15.configure_httpd.png)

### 2.5. Access Website from Browser
To confirm that our Server is configured properly and the website files are in place, open a web browser using the Public IP address of your EC2 instance. This will allow you to view the deployed website.

![Access website](./img/16.access_website_from_browser.png)


## 3. Continuous Integration and Deployment Workflow
For a smooth workflow in developing, testing, and deploying the e-commerce platform, we will follow the following structured approach. It includes making changes in a development environment, using Git for version control, and deploying updates to our production server on AWS.

### Step 1. Developing new features and fixes
- Create and switch to a new branch named `development`.
```sh 
    - git branch development
    - git checkout development
```

I created a new HTML file named `sales.html` and added content to it. 

```sh
vim Sales.html
```

### Step 2. Version Control with Git.

#### 2.1. **Stage the Changes:** After making changes, add them to the staging area in Git.
   ```sh
   git add .
   ```

#### 2.2. **Commit the Changes:** Save the changes in the Git repository with a commit, including a descriptive message.

   ```sh
   git commit -m "Add new features or fix bugs"
   ```

#### 2.3. **Push Changes to GitHub:** Upload the development branch with the new changes to GitHub.

   ```sh
   git push origin development
   ```
![Version Control with Git](./img/17.branch_development.png)

### Step 3. Pull Requests and Merging to the master branch

#### 3.1. Create a Pull Request on GitHub to merge the `development` branch into `master`. This process is crucial for code review and maintaining code quality.

#### 3.2. **Review and Merge the PR:** Review the changes for any potential issues. Once satisfied, merge the pull request into the master branch, incorporating the new features or fixes into the production codebase.

   ```sh
   git checkout master
   git merge development
   ```

#### 3.3. **Push the Merged Changes to GitHub:** Ensure that your local master branch, now containing the updates, is pushed to the remote repository on GitHub.

   ```sh
   git push origin master
   ```
![PR and merging into master branch](./img/18.git_version.png)

### Step 4. Deploying Updates to the Production Server

#### 4.1. **Pull the Latest Changes on the Server:** SSH into your AWS EC2 instance where the production website is hosted. Navigate to the website's directory and pull the latest changes from the master branch.
   ```sh
   git pull origin master
   ```

#### 4.2. **Restart the Web Server (if necessary):** Depending on the nature of the updates, you may need to restart the web server to apply the changes.
   ```sh
   sudo systemctl reload httpd
   ```

### Step 5. Testing the new changes
- **Access the Website:** Open a web browser and navigate to the public IP address of the EC2 instance. Test the new features or fixes to ensure they work as expected in the live environment.

This workflow emphasizes best practices in software development and deployment, including branch management, code review through pull requests, and continuous integration/deployment strategies. By following these steps, you maintain a stable and up-to-date production environment for your e-commerce platform.

![New changes](./img/update.png)

## 6. Troubleshooting

### Git Issues
- **Problem:** Unable to push to GitHub due to authentication failure.
    - **Solution:** Ensure the SSH key is correctly added to your GitHub account, and verify that the correct remote URL is set in Git.

### AWS EC2 Connection Issues
- **Problem:** Cannot SSH into the EC2 instance.
    - **Solution:** Verify that your security group allows inbound SSH traffic on port 22 and that you are using the correct key pair.

### Web Server Issues
- **Problem:** Website does not load after deployment.
    - **Solution:** Check if Apache (httpd) is running using `sudo systemctl status httpd`. Restart if necessary with `sudo systemctl restart httpd`.

- **Problem:** "403 Forbidden" error on accessing the site.
    - **Solution:** Ensure that your file permissions in `/var/www/html` allow public access. Use the following command:

        ```sh
        sudo chmod -R 755 /var/www/html
        ```

### Continuous Integration Issues
- **Problem:** Merge conflicts when merging `development` into `master`.
    - **Solution:** Resolve conflicts manually in the code editor, stage the resolved files, and commit the merge.

- **Problem:** The `sales.html` page and related image are not displaying correctly after an update.
- **Solution:** Ensure that `sales.html` is correctly coded and placed in the appropriate directory, along with the image files in the designated directory:

`MarketPeak_Ecommerce/2127_little_fashion/images`


After verifying these locations, update the web server’s directory and reload `httpd` to apply changes:

```sh
sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/
sudo systemctl reload httpd
```