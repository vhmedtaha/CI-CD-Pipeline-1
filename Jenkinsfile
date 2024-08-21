pipeline{
    agent any 
    tools{
        maven 'maven'
        jdk 'jdk'
    }
    stages{
        stage('fetch code from github'){
            steps{
                git branch: 'main', url: 'https://github.com/hkhcoder/vprofile-project.git'
            }
        }
        stage('build'){
            steps{
                sh 'mvn install -DskipTests'
            }
            post{
                success{
                    echo 'build success'
                    archiveArtifacts artifacts: 'target/*.war'
                }
            }
        }
        stage('test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('checkstyle'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('sonarqube analysis'){
            environment{
                scannerHome = tool 'sonar'
            }
            steps{
                withSonarQubeEnv('sonar'){
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }
        }
        stage('quality gate'){
            steps{
                timeout(time: 1, unit: 'HOURS'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('upload artifact to nexus'){
            steps{
                nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: '44.201.113.223:8081',
                  groupId: 'QA',
                  version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                  repository: 'vprofile',
                  credentialsId: 'nexus',
                  artifacts: [
                    [artifactId: 'vproapp',
                     classifier: '',
                     file: 'target/vprofile-v2.war',
                     type: 'war']
    ]
 )
            }
        }
    }
}
