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
                    def mvnHome = tool name: 'MAVEN3', type: 'maven'
                    def jdkHome = tool name: 'JDK17', type: 'jdk'

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

        stage('SonarQube Analysis') {
            environment {
                SCANNER_HOME = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('sonar') {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=my-java-project \
                        -Dsonar.projectName=MyJavaProject \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=src \
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

    }  // <-- closes stages

}  // <-- closes pipeline
