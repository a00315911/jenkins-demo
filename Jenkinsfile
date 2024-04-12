pipeline{
    agent any
    tools{
        maven "maven3"
    }
    
    environment {
        SONAR_TOKEN = credentials('jenkins-demo')
       
    }
    
    stages{
        stage('Build'){
            steps{
                git 'https://github.com/a00315911/jenkins-demo'
                bat "mvn clean package -DskipTests"
            }
            
            post{
                always{
                    echo 'stage post always'
                }
                
                
                success{
                    echo 'pipeline post success'
                }
                
                failure{
                    echo 'pipeline post failure'
                }
                
            }
        }
        
        stage('Test') {
            steps {
                bat "mvn test jacoco:prepare-agent jacoco:report"
            }

            post {
                always {
                    jacoco(execPattern: '**/target/jacoco.exec')
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                // Avoid redundant work
                bat  "cd jenkins-demo1 && mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN -Dsonar.host.url=http://localhost:9000"
            }
        }
        
    }
}