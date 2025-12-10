pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/skdevops888/Java-Projects-Collections.git', branch: 'patch-1'
            }
        }

        stage('Setup Tools') {
            steps {
                echo 'Setting up Maven and JDK'

                script {
                    // Tool installation paths configured in Jenkins settings
                    def mvnHome = tool name: 'MAVEN3', type: 'maven'
                    def jdkHome = tool name: 'JDK17', type: 'jdk'

                    // Export environment variables
                    env.MAVEN_HOME = mvnHome
                    env.JAVA_HOME  = jdkHome
                    env.PATH = "${jdkHome}/bin:${mvnHome}/bin:${env.PATH}"
                }

                sh 'java -version'
                sh 'mvn -version'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
