pipeline{
    
    agent {
        node {
            label"dev"
        }
    }
    stages{
        stage("Code"){
            steps{
                git url:"https://github.com/pratham-mesh/django-notes-app-jenkins.git",branch:"main"
               
            }
        }
        stage("Build & Test"){
            steps{
                
                sh "docker build . -t notes-app-jenkins:latest"
                
            }
        }
        
        stage("Push to docker hub"){
            steps{
                
                withCredentials(
                    [usernamePassword(
                        credentialsId:"docker-creds",
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubuser"
                        )
                    ]
                ){
                sh "docker image tag notes-app-jenkins:latest ${env.dockerHubuser}/notes-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubuser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubuser}/notes-app-jenkins:latest"
                 }
            }
        }   
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
                
            }
        }
    }   
}
