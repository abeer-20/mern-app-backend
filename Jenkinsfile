pipeline{
    environment {
        imagename = "abeerab/image"
        registryCredential = "dockerhub_credentials"
        scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
        // scannerHome = tool 'sonarqube-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }
    agent any
    tools {
<<<<<<< HEAD
        nodejs "node-js"
=======
        nodejs "node 17.7.2"
>>>>>>> c32bc89615aee2a435b665c38abb8f4b899722c3
    }
    stages{
        stage("test-sonar"){
            steps{
                script {
<<<<<<< HEAD
                    withSonarQubeEnv("sonarQube") {
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=mern-backend\
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://15.236.24.78:9000/ \
=======
                    withSonarQubeEnv("sonarQube") {                  
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=mern-backend\
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
>>>>>>> c32bc89615aee2a435b665c38abb8f4b899722c3
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
<<<<<<< HEAD
                sh 'kubectl --token $api_token --server https://192.168.58.2:8443 --insecure-skip-tls-verify=true apply -f ./kubernetes '
=======
                sh 'kubectl --token $api_token --server http://127.0.0.1:37919 --insecure-skip-tls-verify=true apply -f ./kubernetes '
>>>>>>> c32bc89615aee2a435b665c38abb8f4b899722c3
                }
            }
        }
    }
<<<<<<< HEAD
}
=======
}
>>>>>>> c32bc89615aee2a435b665c38abb8f4b899722c3
