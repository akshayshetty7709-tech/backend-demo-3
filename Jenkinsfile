pipeline {
    agent any
 
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-cred',
                    url: 'https://github.com/Locate360-Project/Locate360-Backend.git'
            }
        }
 
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-locate-360']) {
                    sh '''
                        ssh -A -o StrictHostKeyChecking=no ubuntu@13.202.71.189 \
                            "set -e && \
                            rm -rf /home/ubuntu/Locate360-Backend && \
                            git clone git@github.com:Locate360-Project/Locate360-Backend.git /home/ubuntu/Locate360-Backend && \
                            cd /home/ubuntu/Locate360-Backend && \
                            chmod +x clone.sh && \
                            ./clone.sh && \
                            docker compose down || true && \
                            docker system prune -f && \
                            docker compose up --build -d"
                    '''
                }
            }
        }
    }
}
