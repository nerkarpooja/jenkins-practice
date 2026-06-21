pipeline {

    agent any 

    environment {
        SERVER_IP = '172.31.47.74'
        SSH_KEY = 'target-private-key'
        USER = 'ubuntu'
        REMOTE_DIR = '/var/www/html/'
        REPO_URL = 'https://github.com/nerkarpooja/jenkins-practice.git'
    }
      
    stages {
        stage ('clone the repo on target ')  {
            steps {
                git branch: "main", url: "${REPO_URL}"
            }
        }
        
        
        stage ('ssh into target server '){
            steps {
                sshagent([SSH_KEY]) {
                     sh """
                         ssh -o StrictHostKeyChecking=no ${USER}@${SERVER_IP} '
                             sudo apt update &&
                             sudo apt install -y nginx &&
                             sudo systemctl start nginx &&
                             sudo systemctl enable nginx &&
                             sudo chown -R ubuntu:ubuntu /var/www/html/
                         '
                     """
                }

            }
        }

        stage ('copy index.html to /var/www/html') {
              steps {
                  sshagent([SSH_KEY]) {
                        sh """
                            scp -o StrictHostKeyChecking=no jenkins-practice/index.html ${USER}@${SERVER_IP}:${REMOTE_DIR}
                        """
                    }
                }
            }
        
    }

}