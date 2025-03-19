pipeline {
    agent any

    environment {
        MAVEN_HOME = "/usr/share/maven"
        TOMCAT_USER = "admin"
        TOMCAT_PASSWORD = "admin"
        TOMCAT_URL = "http://13.201.194.180:8080/manager/text"
    }

    triggers {
        pollSCM('H/5 * * * *')  // Poll SCM every 5 mins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'git_cred', 
                    url: 'https://github.com/Ujwal-01/maven-project-02.git', 
                    branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = "target/01-maven-webapp.war"
                    sh """
                        curl -v --user ${TOMCAT_USER}:${TOMCAT_PASSWORD} \\
                        --upload-file ${warFile} \\
                        "${TOMCAT_URL}/deploy?path=/your-app&update=true"
                    """
                }
            }
        }
    }
}
