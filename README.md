# opsschool-ci-cd-lesson1

Welcome to our CI/CD session.
Here you will have instructions on how to create the Jenkins environment.
And the code you will have to deploy.


# Install Jenkins master
* Step one:
```
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

* Step two:
After installation save the temporary password to activate Jenkins initially.
It can be also retrieved from here:
```
/var/lib/jenkins/secrets/initialAdminPassword
```
Navigate to "http://<your instance/vm ip>:8080" and initiate the Jenkins master.

* Step three:
Select "Suggested Plugins" installation type.

* Step four:
Create yourself an admin account.


# Configure a single SSH slave for Jenkins
* Step one:
1. Navigate to "Manage Jenkins"
2. "Manage Plugins"
3. "Install SSH Agent Plugins"
4. After reload, go to "Credentials" and select all providers and types.
5. "Manage Nodes"
6. Add an SSH Node
7. Fill out Credentials
Further examples for setup:
https://wiki.jenkins.io/display/JENKINS/SSH+Slaves+plugin
8. Install Java on slave if needed.
https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get

Voila!

#For running the tests
