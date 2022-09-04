# Configure Jenkins, connect external server to Jenkins as agent and connect 1st repository

## Configure external server for agent launch:
1. Install docker according to your OS.

2. Create jenkins user:
```
sudo useradd -d /home/jenkins jenkins
sudo mkdir /home/jenkins
sudo chown -R jenkins:jenkins /home/jenkins
sudo usermod -a -G docker $USER
sudo mkdir /home/jenkins/.ssh
sudo su - jenkins
```

2. Generate ssh key on server with future jenkins agent.
```
ssh-keygen -f ~/.ssh/jenkins_agent_key
```

3.  Add jenkins_agent_key.pub to authorized_keys (will be needed later)
```
cat /home/jenkins/.ssh/jenkins_agent_key.pub >> /home/jenkins/.ssh/authorized_keys
```

4. Install the necessary packages (centos example)
```
sudo yum -y install java-1.8.0-openjdk git
```

5. Determine java path: (will be needed later)
```
whereis java
```

Major steps have been got from this [article](https://acloudguru.com/blog/engineering/adding-a-jenkins-agent-node)

## Confugure Jenkins server:
1. Launch local jenkins in case you want to test some features:
```
docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker jenkins/jenkins:2.346.2-lts-jdk11
```

2. Install plugins:
Jenkins Dashboard –> Manage Jenkins –> Manage Plugins
```
- docker
- Docker pipeline
```

3. Add agent private ssh key to the Jenkins credentials (Was generated on the step 3 agent configuration block)

Jenkins Dashboard –> Manage Jenkins –> Manage Credentials -> Stores scoped to Jenkins (global) --> Add credentials
```
Kind: SSH Username with private key
Scope: Global
ID: jenkins
Username: jenkins
Private Key: Enter directly --> Add private key (Was generated on the step 3 agent configuration block)
```

4. Add agent on jenkins
Jenkins Dashboard –> Manage Jenkins –> Manage Nodes and Clouds -> New Node -> Type: Permanent agent

```
Remote root directory: /home/jenkins
Labels: {your-custom-lable} (will be use during deploy in Jenkinsfile)
Usage: Only build jobs with label expressions matching this node
Launch method: Launch agents via SSH
  Host: domain or IP
  Credentials: Credentials generated on the previous step. (Step 3)
  Host Key Verification Strategy: Non verifying Verification Strategy
  Advanced:
    Port: 22
    JavaPath: keep blank or use value from step 5 agent configuration.
```

[Credentials configuration](https://www.jenkins.io/doc/book/using/using-agents/#create-a-jenkins-ssh-credential) and [setup agent on Jenkins](https://www.jenkins.io/doc/book/using/using-agents/#setup-up-the-agent1-on-jenkins) steps have been got from this [article](https://www.jenkins.io/doc/book/using/using-agents/)

## Connect github repository to the Jenkins build

1. Create github private repository

2. Generate New access token woth read rights to the repository:
```
https://github.com/settings/tokens
```

3. Test access with new token:
```
cd /folder/with/github/progect
git ls-remote -h -- https://{TOKEN}@github.com/{USERNAME}/repo-name HEAD
```

4. Add github token to the Jenkins credentials :

Jenkins Dashboard –> Manage Jenkins –> Manage Credentials -> Stores scoped to Jenkins (global) --> Add credentials
```
Scope: Global (Jenkins, nodes ...)
User: jenkins-user
Password: Token from previous step
ID: {your-repo-name-access}
Description: {description}
```

5. Configure Jenkinsfile inside of the root repository folder:
<details><summary>Jenkinsfile example</summary>
<p>

```
pipeline {
    agent {
        docker {
            image 'ubuntu:latest'
            label '{your-custom-lable}'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo $PWD'
            }
        }
    }
}
```

</p>
</details>

6. Configure New Jenkins Job:
Jenkins Dashboard –> New Item --> Pipeline
```
Pipeline
Definition: Pipeline from SCM
SCM: Git
Repository URL: {your-repo-url}
Credentials: Credentials created on the step 4
Branch Specifier: */master (or any)
Script Path: Jenkinsfile
Lightweight checkout: check
```

7. Build job:
Jenkins Dashboard –> Project Name -> Build now