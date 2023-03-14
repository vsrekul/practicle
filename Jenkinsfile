pipeline{
    agent any
    stages{
        stage("checkout the source code"){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vsrekul/practicle.git']]])
            }
        }
        stage("Create service from image"){
            steps{                    
              sshagent(['ssh_id']) {
                    sh 'ssh -t -t ubuntu@ec2-3-237-92-117.compute-1.amazonaws.com -o StrictHostKeyChecking=no apt-get install httpd'
                    
              }                
                
            }       
        }
        stage("connect to the remote host"){
            steps{
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh_id', keyFileVariable: 'key', usernameVariable: 'ubuntu')]) {
                    sh 'scp -i ${key} index.html ubuntu@ec2-3-237-92-117.compute-1.amazonaws.com:/var/www/html'
                }
            }
        }
    }
}