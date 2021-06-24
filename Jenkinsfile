pipeline {
   agent any
  
  environment {

      sonar_url = 'http://172.31.26.139:9000/'
      sonar_username = 'admin'
      sonar_password = 'admin'
      nexusUrl = '172.31.26.139:8081'
      artifact_version = '0.0.1'

 }

   tools {
      // Install the Maven version configured as "M3" and add it to the path.
	  jdk 'java8'
      maven "Maven3.3.9"
   }
   
   stages
   {
   stage('git clone') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/snehitha-reddy/Game.git'
        }
        
        }
	stage ('Compile and Build') {
         steps {
           sh '''
           mvn clean install -U  -Dmaven.test.skip=true 
           '''
         }
	}
	stage ('Sonarqube Analysis'){
           steps {
           withSonarQubeEnv('sonarqube') {
           sh '''
           mvn clean package org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=false
           mvn -e -B sonar:sonar -Dsonar.java.source=1.8 -Dsonar.host.url="${sonar_url}" -Dsonar.login="${sonar_username}" -Dsonar.password="${sonar_password}" -Dsonar.sourceEncoding=UTF-8
           '''
           }
         }
      }
	stage ('Publishing Artifact') {
	steps {
	nexusArtifactUploader artifacts: [[artifactId:'gameoflife', classifier: '', file: '/var/lib/jenkins/workspace/pipeline-test/gameoflife-build/target/gameoflife-build-1.0-SNAPSHOT.jar', type:'jar', type: 'jar']], credentialsId: '6034c3f9-74dc-4a2e-b894-46957b8a75d8', groupId: 'com.wakaleo.gameoflife', nexusUrl: '172.31.26.139:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'hold', version: '4.0.0'
           archiveArtifacts '**/*.jar'
	
	
	}
	}
	   stage ('Docker image publish to ECR') {
         steps {
           sh '''
	  docker push 781939683518.dkr.ecr.us-east-2.amazonaws.com/docker:latest
	  
           '''
         }
	}
         }
	}
