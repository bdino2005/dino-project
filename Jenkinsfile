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
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'

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
                scannerHome = tool "${SONARSCANNER}"
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
          stage("My stage") {            
        steps {
            bat label: 'My batch script',
                script: ''' @echo off
                            return_1_if_success.exe   // command which returns 1 in case of success, 0 otherwise
                            IF %ERRORLEVEL% EQU 1 (exit /B 0) ELSE (exit /B 1)'''
        }
    }     
}