<div>
  <h1 align="center"><b>DevSecOps : Netflix Clone CI-CD with Monitoring | Email</b></h1>
  <a href="http://netflix-clone-with-tmdb-using-react-mui.vercel.app/">
    <img src="./public/Netflix-Clone.jpg" alt="Netflix-Clone">
  </a>
  <br /><br />
  <p>
    <b>Hello friends, we will be deploying a Netflix clone. We will use Jenkins as a CI/CD tool and deploy our application in a Docker container and on a Kubernetes cluster. Additionally, we will monitor the Jenkins and Kubernetes metrics using Grafana, Prometheus, and Node Exporter. I hope you find this detailed blog useful.</b>
  </p>
  <h4>
      <b>
        <u>
          <a href="https://github.com/Shravankumar1989/Netflix-clone">
            Click here for the GitHub repository.
          </a>
        </u>
      </b>
  </h4>
  <h4>
    Please follow the steps below.
  </h4>
  <p><b>Step 1 - </b>Launch an Ubuntu (22.04) T2 Large instance.</p>
  <p><b>Step 2 - </b>Install Jenkins, Docker, and Trivy. Create a SonarQube container using Docker.</p>
  <p><b>Step 3 - </b>Create a TMDB API key.</p>
  <p><b>Step 4 - </b>Install Prometheus and Grafana on the new server.</p>
  <p><b>Step 5 - </b>Install the Prometheus plugin and integrate it with the Prometheus server.</p>
  <p><b>Step 6 - </b>Set up email integration with Jenkins and plugin setup.</p>
  <p><b>Step 7 - </b>Install plugins like JDK, SonarQube Scanner, Node.js, and OWASP Dependency Check.</p>
  <p><b>Step 8 - </b>Create a pipeline project in Jenkins using a declarative pipeline.</p>
  <p><b>Step 9 - </b>Install OWASP Dependency Check plugins.</p>
  <p><b>Step 10 - </b>Build and push Docker images.</p>
  <p><b>Step 11 - </b>Deploy the image using Docker.</p>
  <p><b>Step 12 - </b>Set up Kubernetes master and worker on Ubuntu (20.04).</p>
  <p><b>Step 13 - </b>Access the Netflix app in the browser.</p>
  <p><b>Step 14 - </b>Terminate the AWS EC2 instances.</p>
  <h4>
   Now, let's get started and dig deeper into each of these steps:
  </h4>
  <h2><b>Step 1 - Launch an Ubuntu (22.04) T2 Large instance.</b></h2>
  <p><b>Launch an AWS T2 Large instance, using Ubuntu as the image. You can either create a new key pair or use an existing one. In the Security Group, enable HTTP and HTTPS settings, and open all ports (although it's not the best practice to open all ports, it's acceptable for learning purposes).</b></p>
  <p>
    <img src="./public/Screenshot 2023-11-19 004518.png" alt="Screenshot 2023-11-19 004518.png">
    <br /><br />
    <img src="./public/Screenshot 2023-11-19 005041.png" alt="Screenshot 2023-11-19 005041.png">
  </p>
  <h2><b>Step 2 - Install Jenkins, Docker, and Trivy. Create a SonarQube container using Docker.</b></h2>
  <h3><b>Step 2.1 - To Install Jenkins</b></h3>
  <p>Connect to your console and enter these commands to install Jenkins.</p>
  

```sh
#Make sure to run as root, or add [the command] to the user data during the EC2 instance launch.

#Create a shell script named jenkins.sh.
vi jenkins.sh

#Add the code below into the jenkins.sh file.

#!/bin/bash
# Update the package list
sudo apt update -y

# The following line is commented out. If uncommented, it would upgrade all packages.
#sudo apt upgrade -y

# Download and add the Adoptium GPG key for package verification
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc

# Add the Adoptium repository to the sources list
echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list

# Update the package list again after adding new repository
sudo apt update -y

# Install the Temurin 17 JDK package
sudo apt install temurin-17-jdk -y

# Check the Java version to confirm installation
/usr/bin/java --version

# Download and add the Jenkins GPG key for package verification
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add the Jenkins repository to the sources list
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update the package list again after adding the Jenkins repository
sudo apt-get update -y

# Install Jenkins
sudo apt-get install jenkins -y

# Start the Jenkins service
sudo systemctl start jenkins

# Check the status of the Jenkins service
sudo systemctl status jenkins
```

```sh
# Grant all users read, write, and execute permissions on the jenkins.sh file
sudo chmod 777 jenkins.sh

# Execute the jenkins.sh script
./jenkins.sh
```


<p>Once Jenkins is installed, you will need to go to your AWS EC2 Security Group and open inbound port 8080, as Jenkins operates on this port. <br/>Then, obtain your public IP address.</p>



```sh
<EC2 Instance Public IP Address:8080>
#Example
https://13.229.211.33:8080

# Display the initial admin password for Jenkins, stored in the specified file
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
</div>
