pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
               sh 'pipenv --python python3 sync'
            }
        }
        stage('Test') {
            steps {
               sh 'pipenv run pytest'
            }
        }
        stage('Package') {
            when{
                anyOf{ branch "main" ; branch 'release' }
            }
            steps {
               sh 'zip -r sbdl.zip lib'
            }
        }
        stage('Release') {
                when{
                    branch 'release'
                }
                steps {
                    sh "scp -i /home/ubuntu/cred/Jenkins.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf ubuntu@3.133.109.90:/home/ubuntu/sbdl-qa"
                }
                }
        stage('Deploy') {
            when{
                branch 'main'
            }
                steps {
                    sh "scp -i /home/ubuntu/cred/Jenkins.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf ubuntu@3.133.109.90:/home/ubuntu/sbdl-prod"
                }
        }
    }
}