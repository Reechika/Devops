pipeline {
    
    agent none
    
    stages {
        
        stage('1. Install Docker') {
            agent {
                 label 'master'
            }
            environment{
               ANSIBLE_CONFIG = '/usr/lib/python3/dist-packages/ansible/galaxy/data/container/tests' 
            }
            steps {
               sh 'ansible-playbook ansible.yml'
            }
        }
        
        stage('2. Deploy php-app') {
            agent {
                 label 'test-server'
            }
            steps {
                 git 'https://github.com/Reechika/Devops-Project-I.git'
                 sh 'docker build -t php-app:${BUILD_NUMBER} .'
                 sh 'docker run -itd -p 8081:80 --name my-php-app-${BUILD_NUMBER} php-app:${BUILD_NUMBER}'
            }
            post {
                success {
                    echo "Container deployment successful!"
                }
                failure {
                    sh 'docker stop my-php-app-${BUILD_NUMBER}'
                    sh 'docker rm my-php-app-${BUILD_NUMBER}'
                }
            }
            
        }
        
    }
}

