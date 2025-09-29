pipeline {
    agent any

    tools {
        maven "Maven"
        jdk "Java-21"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "Cleaning Workspace......"
                deleteDir()
            }
        }

        stage('Checkout') {
            steps {
                git branch: "main", url: 'https://github.com/venkypv168-ship-it/java-war-repo.git'
            }
        }

        stage('Build') {
            steps {
                echo "Compiling Maven project..."
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo "Running Maven tests..."
                sh 'mvn test'
            }
        }    

        stage('Deploy') {
            steps {
                echo "Deploying the Artifact..."
                //sh 'scp -i /home/ubuntu/Jenkins.pem /home/ubuntu/workspace/Project_9/target/hello-1.0.war ubuntu@3.80.92.210:/home/ubuntu/apache-tomcat-9.0.109/webapps'

            }
        }
    } // End of stages

    post {
        success {
            echo "✔️BUILD AND TEST STAGE SUCCESSFUL...!!!"
            // Updated: allow empty results to prevent failure if no tests
            junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
            archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
        }

        failure {
            echo "❌Failure..!!!"
            mail to: 'venkypv168@gmail.com',
                 subject: "Failed: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                 body: "Job '${env.JOB_NAME}' (${env.BUILD_URL}) failed"
        }

        always {
            echo "🔸🔸🔸You are executed the build🔸🔸🔸!!!"
        }
    }
}
