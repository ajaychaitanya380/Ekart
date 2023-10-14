pipeline {
    agent any
    tools{
        jdk  'jdk17'
        maven  'maven3'
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', changelog: false, credentialsId: '15fb69c3-3460-4d51-bd07-2b0545fa5151', poll: false, url: 'https://github.com/ajaychaitanya380/Ekart.git'
            }
        }
        
        stage('COMPILE') {
            steps {
                sh "mvn clean compile -DskipTests=true"
            }
        }

        stage('TRIVI FS SCAN') {
            steps {
                sh "trivy fs  ."
            }
        }
        
       
        
        
        
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }

        stage('Deploy to Nexus') {
            steps {
                withMaven(globalMavenSettingsConfig: 'globa-settings-xml' ) {
    // some block
                 sh "mvn clean package -DskipTests=true"
                }

            }
        }
        
        stage('Build & Tag Docker Image') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t shopping-cart -f docker/Dockerfile ."
                        sh "docker tag  shopping-cart kar98z/shopping-cart:latest"
                        sh "docker push adijaiswal/shopping-cart:latest"

                    }
                    
                        
                    }
                }
            }
            stage('Docker Push') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push kar98z/shopping-cart:latest"

                    }
                    
                        
                    }
                }
            }
        }
        
        
    }
}