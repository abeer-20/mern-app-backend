pipeline{
    environment {
        imagename = "abeerab/image"
        registryCredential = "dockerhub_credentials"
        scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
        // scannerHome = tool 'sonarqube-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }
    agent any
    tools {

        nodejs "node-js"

        nodejs "node 17.7.2"

    }
    stages{
        stage("test-sonar"){
            steps{
                script {

                    withSonarQubeEnv("sonarQube") {                  
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=mern-backend\
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000/ \
                        -Dsonar.login=admin \
                        -Dsonar.password=123456789"
                    } 
                }
            }
        }
        stage("install dependencies"){
            
            steps{
                sh 'npm install'
            }
        }
        
        stage("docker-build"){
            steps{
                script {
                    dockerImage = docker.build imagename   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
        stage("deployment"){
            steps {
                withCredentials([
                string(credentialsId: 'k8s', variable: 'api_token')
                ]) {

                sh 'kubectl --token $api_token --server http://127.0.0.1:37919 --insecure-skip-tls-verify=true apply -f ./kubernetes '

          
                }
            }
        }
    }

}
