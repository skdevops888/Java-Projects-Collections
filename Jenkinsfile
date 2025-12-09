node {

    stage('Checkout') {
        checkout scm
    }

    stage('Setup Tools') {
        echo "Setting up Maven and JDK"

        env.MAVEN_HOME = tool 'maven-3.8.6'   // Use your Maven tool name
        env.JAVA_HOME  = tool 'jdk11'         // Use your JDK tool name
        env.PATH = "${env.MAVEN_HOME}/bin:${env.JAVA_HOME}/bin:${env.PATH}"

        sh "mvn -version"
        sh "java -version"
    }

    stage('Build ATM Project') {
        sh """
            cd ATM
            mvn clean package -DskipTests
        """
    }

    stage('SonarQube Analysis') {
        withSonarQubeEnv('sonar') {
            sh """
                cd ATM/ATM
                mvn clean verify sonar:sonar \
                -Dsonar.projectKey=ATM-Project \
                -Dsonar.projectName='ATM Java Project'
            """
        }
    }

    stage('Quality Gate') {
        timeout(time: 2, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }

    stage('Archive Artifact') {
        archiveArtifacts artifacts: 'ATM/ATM/target/*.jar', fingerprint: true
    }
}
