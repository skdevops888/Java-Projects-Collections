node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build ATM Project') {
        sh "cd ATM/ATM && mvn clean package -DskipTests"
    }

    stage('SonarQube Analysis') {
        withSonarQubeEnv('sonar') {    // sonar = name configured in Jenkins
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
