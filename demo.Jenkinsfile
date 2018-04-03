pipeline {
    agent any
    tools {
        maven 'Maven_home'
        jdk 'Java_home'
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war' , fingerprint: true
                }
            }
        }

        stage('Deploy-to-staging'){
            steps {
                sh '''
                curl -s --upload-file **/target/*.war "http://tomcat:tomcat@http://18.218.67.102:8090/manager/text/deploy?path=/myapp&update=true&tag=${BUILD_TAG}
                '''
            }
            post {
                success {
                    echo 'Now Deployed...'
                }
            }
        }
    }
}
