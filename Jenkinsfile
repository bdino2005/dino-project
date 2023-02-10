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
        NEXUS_GRP_REPO = 'dino-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        SONNARSERVER = 'sonarserver'
        SONNARSCANNER = 'sonarscanner'

    }
    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'   
            }
            post {
                success {
                    echo "now archiving"
                    archiveArtifacts artifacts: '**/*.war'
                }
            }  
        }
        stage('test'){
            steps {
                sh 'mvn -s settings.xml test'
            }
        }
        stage('checkstyle analysis'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
         stage('Sonar Analysis') {
            environment {
                scannerHome = "${SONARSCANNER}"
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=dino-project \
 
                   -Dsonar.projectName=dino-project \

                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }
        }
    }
}