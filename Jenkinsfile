pipeline {
  environment {
    registry = 'hohioh/demo3'
    registryCredential = 'dockerHub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Mail Notification') {
      steps {
        echo 'Sending Mail'
        mail bcc: '',
        body: 'Jenkins Build Started',
        cc: '',
        from: '',
        replyTo: '',
        subject: 'Jenkins Job',
        to: 'example@domain.com'
      }
    }
    stage('Git') {
      steps {
        echo 'Cloning'
        git branch: 'spring-for-jenkins-with-docker', url: 'https://github.com/nadernext/devops-pipeline.git'
      }
    }
    stage('MVN CLEAN') {
      steps {
        echo 'Maven Clean'
        sh 'mvn clean'
      }
    }
    stage('MVN TEST JUNIT') {
      steps {
        echo 'Maven Test JUnit'
        sh 'mvn test'
      }
    }
    stage('MVN TEST SONAR') {
      steps {
        echo 'Sonar Test Code Quality'
        sh 'mvn sonar:sonar -Dsonar.host.url=http://sonarqube:9000'
      }
    }
    stage('MVN PACKAGE') {
      steps {
        echo 'Maven Packaging'
        sh 'mvn package -Dmaven.test.skip=true'
      }
    }
    stage('NEXUS') {
      steps {
        echo 'Nexus Packaging'
        sh 'mvn clean package -Dmaven.test.skip=true deploy:deploy-file -DgroupId=tn.esprit.spring -DartifactId=timesheet-devops -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar  -DrepositoryId=sonartypeNexusRepo -Durl=http://nexus3:8081/repository/maven-releases/ -Dfile=target/timesheet-devops-1.0.jar'
      }
    }
    stage('Building our image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy image to Docker Hub') {
      steps {
        script {
         // docker.withRegistry('', registryCredential) {
         //   dockerImage.push()
          }
        }
      }
    }
    stage('Cleaning up Docker Image') {
      steps {
        // sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
      post {
        success {
        echo 'whole pipeline successful'
        mail bcc: '',
        body: "Project with name ${env.JOB_NAME}, with Build Number: ${env.BUILD_NUMBER}, was built successfully. to check the build result go to build URL: ${env.BUILD_URL}",
        cc: '',
        from: '',
        replyTo: '',
        subject: 'Build success',
        to: 'example@domain.com'
        }
        failure {
        echo 'pipeline failed, at least one step failed'
        mail bcc: '',
        body: "there was an error in  ${env.JOB_NAME}, with Build Number: ${env.BUILD_NUMBER} to check the error go to build URL: ${env.BUILD_URL}",
        cc: '',
        from: '',
        replyTo: '',
        subject: 'Build Failure',
        to: 'example@domain.com'
        }
      }
}
