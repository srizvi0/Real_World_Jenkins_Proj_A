pipeline {
    agent any

    tools {
        jdk 'JDK1'
        maven 'Maven'
    }

    stages {
        stage('Git-Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/srizvi0/Real_World_Jenkins_Proj_A.git'
            }
        }
        stage('Code-Compile') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Unit-Testing') {
            steps {
                bat 'mvn test'
            }
        }

        stage('SonarCube-Analysis') {
            steps {
                withSonarQubeEnv('sonarserver') {
                     bat '''mvn clean verify sonar:sonar -Dsonar.projectKey=Project1 -Dsonar.projectName=Project1 -Dsonar.java.binaries=. '''
                }
            }
        }    
        stage('OWASP SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ ',  odcInstallation: 'Dependency Check'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }     
        
        stage('build Artifact'){
            steps{
                bat 'mvn clean install'
            }
        }
        
        stage('Docker Build'){
            steps{
                    bat 'docker build -t docker12 .'
            }
        }
        stage('Docker tag & push'){
            steps{
                bat 'docker tag docker12 najamrizvi3/dockerimage0325:latest'
                bat 'docker push najamrizvi3/dockerimage0325:latest'
            }
        }
        
        stage ('Deploy using docker'){
            steps{
                bat 'docker run -d --name deploy1 -p 8085:8085 najamrizvi3/dockerimage0325:latest'
            }
        }
        
        stage('Deploy to tomcat'){
            steps{
                bat 'copy "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\CICD_RealWorld_Project_1\\target\\petclinic.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps"'
                //http://localhost:8081/petclinic/ 
                // /petclinic is from the name of .war file
            }
        }
    }                
}
