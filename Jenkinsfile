pipeline {
    agent any
     tools {
        maven 'Maven'
     }
    stages {
        stage("Test"){
            steps{
                // mvn test
                sh "mvn test"
                slackSend channel: 'youtubejenkins', message: 'Job Started'
                echo "========executing Test========"
            }
        }
        stage("Build"){
            steps{
                sh "mvn package"
                echo "========executing Build========"
            }
        }
        stage("Deploy on Test"){
            steps{ 
                 // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverdetails1', path: '', url: 'http://192.168.0.118:8000')], contextPath: '/app', war: '**/*.war'
                echo "========executing A========"
            }
        }
        stage("Deploy on Prod"){
            input{
                message "Should we continue?"
                ok "Yes we Should"
            }
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverdetails1', path: '', url: 'http://192.168.0.119:8000')], contextPath: '/app', war: '**/*.war'
                echo "========executing A========"
            }
        }
    }
    post{
       always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
             slackSend channel: 'youtubejenkins', message: 'Success'
        }
        failure{
            echo "========pipeline execution failed========"
             slackSend channel: 'youtubejenkins', message: 'Job Failed'
        }
    }
}
