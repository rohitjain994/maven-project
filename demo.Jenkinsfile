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
            def userInput = true
            def didTimeout = false
            try {
                timeout(time: 15, unit: 'SECONDS') { // change to a convenient timeout for you
                    userInput = input(
                    id: 'Proceed1', message: 'Was this successful?', parameters: [
                    [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']
                    ])
                }
            } catch(err) { // timeout reached or input false
                def user = err.getCauses()[0].getUser()
                if('SYSTEM' == user.toString()) { // SYSTEM means timeout.
                    didTimeout = true
                } else {
                    userInput = false
                    echo "Aborted by: [${user}]"
                }
            }

            node {
                if (didTimeout) {
                    // do something on timeout
                    echo "no input was received before timeout"
                } else if (userInput == true) {
                    // do something
                    echo "this was successful"
                    currentBuild.result = 'SUCCESS'
                } else {
                    // do something else
                    echo "this was not successful"
                    currentBuild.result = 'FAILURE'
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
