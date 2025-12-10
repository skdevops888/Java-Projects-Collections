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
 /*  ⭐⭐⭐ ADD SONAR HERE ⭐⭐⭐ */

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

    }
}
⭐ 3. VISUAL DIAGRAM (EASIEST WAY TO UNDERSTAND)
diff
Copy code
Checkout
    ↓
Setup Tools
    ↓
Build      ← YOU ALREADY HAVE THIS
    ↓
--------------------------------------
↓↓ PASTE SONAR STAGE HERE ↓↓
--------------------------------------
    ↓
SonarQube Analysis
    ↓
Quality Gate
    ↓
DONE
⭐ If you want, I can MODIFY your existing Jenkinsfile directly.
Just send me your current file, and I will insert it for you.







You said:
yes


ChatGPT said:
Perfect — I’ll modify your exact Jenkinsfile and insert the SonarQube Analysis stage in the correct place so you don’t need to think about where to paste anything.

Here is your FINAL, COMPLETE, READY-TO-USE JENKINSFILE.
Just copy–paste it into your repo. ✅

⭐⭐ FINAL JENKINSFILE WITH SONARQUBE ADDED (WORKING VERSION) ⭐⭐
groovy
Copy code
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

        /*****************************
         * ⭐ SONARQUBE ANALYSIS ⭐
         *****************************/
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

        /*****************************
         * ⭐ QUALITY GATE ⭐
         * (Waits for Sonar results)
         *****************************/
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

    }
}
