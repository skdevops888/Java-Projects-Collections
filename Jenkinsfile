node {

    stage('Prepare Environment') {
        echo "Setting up JDK and Maven..."

        // Load tools from Jenkins Global Tool Configuration
        env.MAVEN_HOME = tool name: 'maven-3.8.6', type: 'maven'
        env.JAVA_HOME  = tool name: 'jdk11', type: 'jdk'
        env.PATH = "${env.MAVEN_HOME}/bin:${env.JAVA_HOME}/bin:${env.PATH}"

        sh "java -version"
        sh "mvn -version"
    }

    stage('Checkout Code') {
        checkout([$class: 'GitSCM',
            branches: [[name: '*/patch-1']],     // Your branch
            userRemoteConfigs: [[url: 'https://github.com/skdevops888/Java-Projects-Collections.git']]
        ])
    }

    stage('Build Project') {
        sh "mvn clean package -DskipTests"
    }

    stage('Archive Artifact') {
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
    }

    stage('Cleanup Workspace') {
        cleanWs()
    }

}
