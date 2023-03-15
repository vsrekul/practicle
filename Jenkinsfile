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
                    sh 'ssh -t -t ubuntu@ec2-3-237-92-117.compute-1.amazonaws.com -o StrictHostKeyChecking=no sudo apt-get update'
                    //sh 'ssh -t -t ubuntu@ec2-3-237-92-117.compute-1.amazonaws.com -o StrictHostKeyChecking=no sudo apt-get install nginx -y'
                    
              }                
                
            }       
        }
        stage("copy the file to the remote host"){
            steps{
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh_id', keyFileVariable: 'key', usernameVariable: 'ubuntu')]) {
                    sh 'scp -i ${key} index.html ubuntu@ec2-3-237-92-117.compute-1.amazonaws.com:'
                    sh 'scp -i ${key} playbook.yml ubuntu@ec2-3-237-92-117.compute-1.amazonaws.com:'
                }
            }
        }
        stage("move index file in remote host"){
            steps{                    
              sshagent(['ssh_id']) {
                    sh 'ssh -t -t ubuntu@ec2-3-237-92-117.compute-1.amazonaws.com -o StrictHostKeyChecking=no Ansible-playbook playbook.yml'                 
                    
              }                
                
            }       
        }
    }
}