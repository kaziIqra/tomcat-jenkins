pipeline {
    agent any
    
    tools {
        jdk 'jdk17.0.13'
        maven 'Maven'
    }
    
    environment {
        WAR_FILE = 'target/roshambo.war'
        TOMCAT_URL = 'http://localhost:8082'
        TOMCAT_USER = 'kaziqra'
        TOMCAT_PASSWORD = 'kaziqra9867'
    }
    
    stages {
        stage('Clean Project') {
            steps {
                bat "mvn clean" // change to "sh" if on Linux/Mac
            }
        }

        stage('Build Project') {
            steps {
                bat "mvn package" // change to "sh" if on Linux/Mac
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFilePath = "${WORKSPACE}/${WAR_FILE}"
                    echo "WAR file path: ${warFilePath}"
                    
                    if (fileExists(warFilePath)) {
                        echo 'WAR file found, proceeding with deployment...'
                        
                        bat """
                            curl --upload-file "${warFilePath}" \
                            --user "${TOMCAT_USER}:${TOMCAT_PASSWORD}" \
                            "${TOMCAT_URL}/manager/text/deploy?path=/roshambo&update=true"
                        """ // change to "sh" if on Linux/Mac
                    } else {
                        error('WAR file not found! Build might have failed.')
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Build completed'
        }
    }
}
