pipeline{
    agent any;
    
    stages{
        stage("Cloning code"){
            steps{
                echo "code clone ho gya "
                git url: "https://github.com/vishaltyagi178/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build docker image from cloned code"){
         steps{
               echo "code build ho gya " 
               sh "docker build -t flask-app ."
            }   
        }
        stage("Push image to docker hub"){
          steps{
               withCredentials([usernamePassword(
                   credentialsId: "dockerSecret",
                   passwordVariable: "dockerPass",
                   usernameVariable: "dockerUser"
                   )]){
                sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                sh "docker image tag flask-app ${env.dockerUser}/flask-app"
                sh "docker push ${env.dockerUser}/flask-app:latest"
           }
          }
        }
        stage("Deploy container to environment"){
          steps{
                echo "code deploy ho gya"
                sh "docker compose up -d"
            }  
        }
    }
    
}
