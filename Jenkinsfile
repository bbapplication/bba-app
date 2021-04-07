pipeline {
    agent any
    tools {  
        maven "M3"
    }
    stages {
        stage('Build') {
            steps {
               
               git branch: 'main', url: 'https://github.com/bbapplication/bba-app.git'

               // sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar"
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

              
            }
            post {
              
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws_cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        sh "aws s3 cp /home/ubuntu/jenkins-agent/workspace/pipline/target/bba-app-1.0-SNAPSHOT.jar s3://myjenkinsdemo/bba-app/main/"
                    }
                }
            }
        }
    }
}
