pipeline {
    agent none
    tools {
        maven 'Maven_home'
        jdk 'Java_home'
    }
    triggers { 
        pollSCM('* * * * *') 
    }
    stages{
        stage('Build'){
            agent{lable : 'vdinfra'}
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
            agent {
                lable : 'vdinfra'
            }
            timeout(time:5, unit:'MINUTES') {
                input message:'Approve deployment?', submitter: 'it-ops'
            }
            
        }

        stage('Deploy-to-staging'){
            agent{
                lable : 'vdinfra'
            }
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
