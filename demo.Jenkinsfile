pipeline {
    agent any
    tools {
        maven 'Maven_home'
        jdk 'Java_home'
    }
    triggers { 
        pollSCM('* * * * *') 
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

        stage('Staging Permission') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }

        stage('Deploy-to-staging'){
            steps {
                sh '''
                curl -s --upload-file **/target/*.war "http://tomcat:tomcat@18.218.67.102:8090/manager/deploy?path=/webapp&update=true"
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
