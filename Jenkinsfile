pipeline{
    
    agent {label "dev"};
    
    stages{
        
        stage("code clone"){
            
            steps{
                
                git url: "https://github.com/vishaltyagi178/two-tier-flask-app.git", branch: "master"
            }
            
        }
        stage ("build"){
            
            steps{
                
               sh " docker build -t flask-app ."
               
            }
        }
        stage ("test"){
            
            steps{
                
                echo "test team will give results"
                
            }
        }
        
        stage ("push to docker hub"){
            
            steps{
                
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCred",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                        
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker image tag flask-app ${env.dockerHubUser}/flask-app"
                        sh "docker push ${env.dockerHubUser}/flask-app:latest"
                    }
                
            }
        }
            
        stage ("prod"){
            
            steps{
                
                sh "docker compose up -d --build flask-app"
            }
        }
    }
        
     post {
          success{
              script{     
         emailtext from: 'vishaltyagi178@gmail.com',
          to: 'vishaltyagi178@gmail.com',
          body: 'Build success',
          subject: 'Build success',
          }
              
          }
      }
    
    
}
