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
    }
    stages{
        stage("test-sonar"){
            steps{
                script {
                    withSonarQubeEnv("sonarQube") {
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=mern-backend\
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://15.236.24.78:9000/ \
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
                sh 'kubectl --token $api_token --server https://192.168.58.2:8443 --insecure-skip-tls-verify=true apply -f ./kubernetes '
                }
            }
        }
    }
}
