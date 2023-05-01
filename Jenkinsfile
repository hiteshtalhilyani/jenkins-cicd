pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    environment {
        SNAP_REPO = 'webapp-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'webapp-release'
        CENTRAL_REPO =  'webapp-maven-central'
        NEXUSIP = '172.31.39.126'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'webapp-maven-group'
        NEXUS_LOGIN = "nexus-login"
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
        // NEXUS_VERSION = "nexus3"
        // NEXUS_PROTOCOL = "http"
        // NEXUS_URL = "3.110.128.159:8081"
        // NEXUS_REPOSITORY = "webapp-release"
        // NEXUS_REPOGRP_ID    = 
        // NEXUS_CREDENTIAL_ID = "nexus-login"
        // ARTVERSION = "${env.BUILD_ID}"

    }
    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Test') {
            steps{
                sh 'mvn -s settings.xml test'
            }
        }
        stage('Checkstyle Analysis'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }  
        stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool '$(SONARSCANNER)'
          }

          steps {
            withSonarQubeEnv('$(SONARSERVER)') {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }

            timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
            }
          }
        } 
    }
}