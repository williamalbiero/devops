pipeline{
    agent any

    environment{
        URL_REGISTRY_DOCKER = 'localhost:5000'
    }
    stages{

        stage("Build Image Container & Push"){
            when{
                anyOf {
                    changeset 'Dockerfile'
                }
            }
            steps{
              
              script { 
                echo "Building..."
                def rebuildImage = docker.build("infraascode:${env.BUILD_ID}")
                echo "Pushing..."
                
                sh """

                    docker tag infraascode:${env.BUILD_ID} ${env.URL_REGISTRY_DOCKER}/infraascode:${env.BUILD_ID} && docker push ${env.URL_REGISTRY_DOCKER}/infraascode:${env.BUILD_ID}

                   """
               }    
            }
        }

        stage ('Deploy Container'){
            steps{
                sh """
                   
                    docker run -dit -p 8080:8080 --name infraascode ${env.URL_REGISTRY_DOCKER}/infraascode:${env.BUILD_ID}
                   
                   """ 
            }
        }
    }
}