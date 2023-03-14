pipeline{
    agent any
    stages{
        stage(){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vsrekul/practicle.git']]])
            }
        }
    }
}