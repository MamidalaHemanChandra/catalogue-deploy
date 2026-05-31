pipeline {
    agent {
        node {
            label 'Agent-1'
        }
    }

    environment {
        Course = 'Jenkins'
        appVersion = ""
        ACC_ID = "634758830486"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }

    parameters {
        string(name: 'appVersion', description: 'Which app version you want to deploy?')
        choice(name: 'deploy_to', choices: ['dev', 'qa', 'prod'], description: 'Pick something')       
    }

    options {
        timeout(time: 60, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {

        stage('Deploy') {
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """
                        aws eks update-kubeconfig --region us-east-1 --name ${PROJECT}-${params.deploy_to}
                        """
                    }
                }
            }
        }

        
    }

    post {
        always {
            echo 'I will always say Hello again!'
            cleanWs()
        }
        success {
            echo 'I will run if success'
        }
        failure {
            echo 'I will not run if failure'
        }
        aborted {
            echo 'Pipeline is aborted'
        }
    }
}
