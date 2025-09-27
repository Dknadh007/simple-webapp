pipeline {
    agent any

    tools {
        maven 'Maven'       // Name of Maven installation in Jenkins
        jdk 'Java17'        // Name of JDK installation in Jenkins
    }

    environment {
        GIT_CREDENTIALS = 'github-creds'       // Jenkins credential ID for GitHub
        REPO_URL = 'https://github.com/Dknadh007/simple-webapp.git'
        BRANCH = 'main'
        TOMCAT_HOME = '/opt/tomcat'           // Path to Tomcat installation on your server
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning GitHub repo..."
                git branch: "${BRANCH}",
                    credentialsId: "${GIT_CREDENTIALS}",
                    url: "${REPO_URL}"
            }
        }

        stage('Build with Maven') {
            steps {
                echo "Building the project..."
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo "Deploying WAR to Tomcat..."
                sh """
                    cp target/simple-webapp.war ${TOMCAT_HOME}/webapps/
                """
            }
        }

        stage('Post-Deployment') {
            steps {
                echo "Deployment complete. You can access the app at http://<server-ip>:8081/simple-webapp/"
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the console output for errors."
        }
    }
}
