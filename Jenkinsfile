  pipeline{
   agent any 
    
    tools {
        maven 'maven3' 
    }
    environment{
        SONAR_TOKEN = credentials('global_token')
    }
   
    stages {
        stage('Clone Git Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/a00315911/jenkins-demo.git'
            }
        }
        stage('Build Project') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('Execute Unit Tests') {
      		steps {
        		bat 'mvn test'
      		}
    	}
        stage('Static Code Analysis (SonarQube)') {
        	steps {
        		withSonarQubeEnv('SonarQube_server'){
        		 bat 'mvn sonar:sonar'   
        		}
      		}
        }
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts 'target/*.jar'
            }
        }
        stage('Deploy Application (Simulation)') {
            steps {
                echo 'Successfully deployed the application!'
            }
        }
    }
}
