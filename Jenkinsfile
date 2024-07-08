pipeline {
    agent any

    stages {
        stage('Install the dependencies') {
            steps {
                sh "pip install -r requirements.txt"
            }
        }

        stage('Run tests') {
            steps {
               sh "python manage.py test"
            }
        }

        stage('Deploy') {
            steps {
                 withCredentials([sshUserPrivateKey(credentialsId: 'Django-jenkins', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                     sh '''
                        mkdir -p ~/.ssh
                        ssh-keyscan -H 13.201.174.63  >> ~/.ssh/known_hosts
                        scp -i $SSH_KEY -r * $SSH_USER@3.13.201.174.63:/var/www/html
                        ssh -i $SSH_KEY $SSH_USER@3.111.49.36 'sudo systemctl restart httpd'
                     '''
                 }   
            }
        }
    }
}
