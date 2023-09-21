pipeline{
  agent any
    tools{
      mvn 'Maven'
      }
  stages{
    stage('SonarQube scan'){
      steps{
      bat 'mvn sonar:sonar \
           -Dsonar.projectKey=another-java-maven-app \
           -Dsonar.host.url=http://localhost:9000 \
           -Dsonar.login=b179928b5efbd05c78ebe05fb3b49d025d9ef88e'
       }
    }
    stage('Build'){
      steps{
      bat 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test'){
      steps{
      bat 'mvn test'
      }
      post{
      always{
        junit 'target/surefire-reports/*.xml'
        }
      }
    }
    stage('Publish to Artifactory'){
      steps{
        rtMavenDeployer(
          id:'deployer',
          serverid:'Pipeline_repository@artifactory',
          releaseRepo:'Pipeline_repository',
          snapshotRepo:'Pipeline_repository'
        )
        rtMavenRun(
          pom:'pom.xml',
          goals:'clean install',
          deployerId:'deployer'
        )
        rtPublishBuildInfo(
          serverid:'Pipeline_repository@artifactory'
        )
      }
    }
  }
 } 


    
    
    
    
