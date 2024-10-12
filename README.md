# Creating a simple Node.js application code that serves a “Hello World” message.
1 Create a new directory for the application:
mkdir hello-world-app
cd hello-world-app
2 Create a package.json file: This file defines the application's dependencies and configuration.
// https://github.com/SuryaGopilli/Hello-world-app/blob/master/package.json
3 Create the main application file, app.js: This is the code that will run the application.
// https://github.com/SuryaGopilli/Hello-world-app/blob/master/app.js
4 Create a Dockerfile: This file defines how to build the Docker image for the application.
// https://github.com/SuryaGopilli/Hello-world-app/blob/master/dockerfile

# DEVOPS ENGINEER ROLE
1. Pushing locaal code to GITHUB
sudo apt update
sudo apt install git
# As of August 13, 2021, GitHub no longer supports password authentication for Git operations.
//git config --global user.name "Your Name"
//git config --global user.email "your.email@example.com"
# Instead, you can use either Personal Access Tokens (PAT) or SSH keys for authentication for above two lines
cd ~/hello-world-app
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/hello-world-app.git
git push -u origin master
# after push When prompted for your username, enter your GitHub username. When prompted for your password, paste your Personal Access Token(PAT) instead.

# Building, Running, and Testing the Docker Image
Step 1: Build the Docker Image
1. Open your terminal and navigate to the hello-world-app directory:
cd path/to/hello-world-app
2. Build the Docker image using the following command:
docker build -t hello-world-app .
docker images
//The -t hello-world-app flag tags the image with the name hello-world-app.
//The . indicates the current directory contains the Dockerfile.
//Docker images will check the image was created or not.
Step 2: Run the Docker Container
1. Run the Docker container using the following command:
docker run -d -p 3000:3000 hello-world-app
//The -d flag runs the container in detached mode.
//The -p 3000:3000 flag maps port 3000 of the container to port 3000 on your host machine.
Step 3: Test the Application
1. Open your web browser or use a tool like Postman and navigate to:
http://localhost:3000 or curl http://localhost:3000
//You should see the message "Hello, World!" displayed.
//you can also check using curl http://localhost:3000
Step 4: Verify the Application is Running
1. You can also verify that the container is running by executing:
docker ps
// This command lists all running containers. You should see hello-world-app in the list.

# PERMISSION DENIED ERROR : REASON- DIDN'T ADDED UBUNTU USER TO DOCKER GROUP 
ubuntu@ip-172-31-47-2:~/hello-world-app$ docker build -t hello-world-app .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/build?buildargs=%7B%7D&cachefrom=%5B%5D&cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=dockerfile&labels=%7B%7D&memory=0&memswap=0&networkmode=default&rm=1&shmsize=0&t=hello-world-app&target=&ulimits=null&version=1": dial unix /var/run/docker.sock: connect: permission denied
ubuntu@ip-172-31-47-2:~/hello-world-app$
// ADDED USER UBUNTU TO DOCKER GROUP: sudo usermod -aG docker ubuntu
// INITIATED BUILD AGAIN
# package-lock.json FILE DOESN'T EXIST ERROR: REASON - NEED TO INSTALL NPM
Step 4/8 : COPY package-lock.json ./
COPY failed: file not found in build context or excluded by .dockerignore: stat package-lock.json: file does not exist
//INSTALLED NPM PACKAGE FOR package-lock.json FILE : npm install
//ENSURE package-lock.json IS PRESENT IN DIR : CAN BE CHECKED USING ls-la
//INITIATED BUILD AGAIN
//BUILD SUCCESS

# DOCKER REPOSITORY NAME WAS TYPO AS hello-wold-app instead of hello-world-app
use command: docker image
REPOSITORY        TAG       IMAGE ID       CREATED         SIZE
hello-wold-app   latest    a1b2c3d4e5f6   10 minutes ago  150MB
my-new-repo       latest    a1b2c3d4e5f6   10 minutes ago  150MB

// Used the docker tag command to rename (retag) the image:
sudo docker tag hello-wold-app:latest hello-world-app:latest
// ran the image again to build the image with correct name
sudo docker run -d -p 3000:3000 hello-world-app:latest
// removed the old tag
sudo docker rmi hello-wold-app:latest

Step:1 Created an ecr registry (elastic container registry) to push docker image into ecr | we can also use docker hub
Step:2 Created and attached an IAM Role for connected ec2 instance with policy : AmazonEC2containerregistryfullacess
Step:3 Created own json policy for ECR full acess to IAM User
// https://github.com/SuryaGopilli/Hello-world-app/blob/master/Json-policy
Step4: Created an Access key and secret access key to IAM user for Aws configuration

# ERROR OCCURED WHILE CONFIGURING AWS : NEED TO INSTALL AWS CLI 
//Install AWS CLI:
sudo apt-get update
sudo apt-get install awscli
//If the package isn't available, you can install it using the bundled installer:
sudo apt install unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# ERROR OCCRED BECAUSE NOT USING ACCESSKEY : CREATE ACCESSKEY AND SECRET ACCESSKEY FOR IAM USER
//An error occurred (UnrecognizedClientException) when calling the GetAuthorizationToken operation: The security token included in the request is invalid.
//Error: Cannot perform an interactive login from a non TTY device

# ERROR OCCURED WHILE LOGGING INTO ECR, BECAUSE OF USING USERID WITH '-' SPACED : 8517-2526-2025 INSTEAD OF "851725262025" 
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 8517-2526-2025.dkr.ecr.ap-south-1.amazonaws.com
//[cloudshell-user@ip-10-134-35-117 ~]$ aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 8517-2526-2025.dkr.ecr.ap-south-1.amazonaws.com
//Error response from daemon: login attempt to https://8517-2526-2025.dkr.ecr.ap-south-1.amazonaws.com/v2/ failed with status: 400 Bad Request
//[cloudshell-user@ip-10-134-35-117 ~]$ 
# Troubleshooting Steps to loginto ECR
1.Confirm ECR Repository Exists: Ensure that the ECR repository (851725262025) exists in the specified region (ap-south-1).
2.Correct Login Command: aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 851725262025.dkr.ecr.ap-south-1.amazonaws.com
3.Check IAM Permissions: Ensure that the IAM user or role you are using has the necessary permissions to access ECR
4.Check the Docker Daemon is running : sudo systemctl status docker  //ifnot start it using: sudo systemctl start docker

# JENKINS
Step:1 install jenkins
Step:2 install additional plugins - digital ocean, dockerpipeline
Step:3 Add jenkins to docker group :  sudo usermod -aG docker jenkins
Step:4 restart the jenkins: sudo systemctl restart jenkins (or) http://<yourip>:8080/restart
Step:5 Create a pipeline project and select pipeline from scm, provide the url and provide the jenkinsfile path.
Step:6 save it and go to oceanblue and run the pipeline, if builds success go to aws ecr an docker image will be created.

# Jenkinsfile Summary
Agent any
Environment: Aws region and ECr Repo, Tagging image using jenkins build(Image_Tag),ECR URI.
Under stages:
1. Clone the application repository from GitHub.
2. Build the Docker image from the application code.
3. Log into AWS ECR to allow image pushing.
4. Tag the Docker image with the build ID.
5. Push the Docker image to the ECR repository.
6. Display success or error messages based on the outcome.

