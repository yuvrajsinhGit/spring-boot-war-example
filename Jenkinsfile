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
                slackSend channel: 'harsh', message: 'job started ' 
                
            }
            
        }
        stage("Build"){
            steps{
                sh "mvn package"
                
            }
            
        }
        stage("Deploy on Test"){
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'admin123', path: '', url: 'http://13.127.241.151:8081')], contextPath: '/maven', war: '**/*.war'
              
            }
            
        }
        stage("Deploy on Prod"){
             input {
                message "Should we continue?"
                ok "Yes we Should"
            }
            
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'admin123', path: '', url: 'http://13.127.241.151:8081')], contextPath: '/maven', war: '**/*.war'
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
             slackSend channel: 'harsh', message: 'Success'
        }
        failure{
            echo "========pipeline execution failed========"
             slackSend channel: 'harsh', message: 'Job Failed'
        }
    }
}
