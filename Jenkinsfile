pipeline{
    
    agent { lable "dev"};
    
    stages{
        
        stage("code clone"){
            
            steps{
                
                echo "copying code from remote library"
                git url: "https://github.com/vishaltyagi178/two-tier-flask-app.git", branch: "master"
            }
            
        }
        
         stage("Build"){
            
            steps{
                
                echo "building application image"
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        
        stage("Push to Docker Hub"){
            
            steps{
                
                withCredentials([usernamePassword(
                      credentialsId: "dockerCred",
                      passwordVariable: "dockerPass",
                      usernameVariable: "dockerUser"
                    )]){
                 sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                 sh "docker image tag two-tier-flask-app ${env.dockerUser}/two-tier-flask-app"
                 sh "docker push ${env.dockerUser}/two-tier-flask-app:latest"
                    }
            }
            
        }
        stage("Deploy"){
            
            steps{
                
                echo "We are pushing our application to Prod"
                sh "docker compose up -d --build flask-app"
            }
            
        }
    }
    
    
}
