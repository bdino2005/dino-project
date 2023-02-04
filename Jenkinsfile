pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    environment {
        SNAP_REPO = 'dino-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'dino'
        RELEASE_REPO = 'dino-release'
        CENTRAL_REPO ='dino-maven-central'
        NEXUSIP = '172.31.19.178'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = ' dino-maven-group'
        NEXUS_LOGIN = 'nexuslogin'

    }
    stages {
        stage('build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
                
                
            }
        }
    }
}