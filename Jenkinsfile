pipeline {
  agent none
  stages {
    stage('build') {
      when {
        anyOf {
          branch 'main'
          not { branch 'main' }
        }
      }
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17'
        }
      }
      steps {
        echo 'compile maven app'
        sh 'mvn compile'
      }
    }

    stage('test') {
      when {
        anyOf {
          branch 'main'
          not { branch 'main' }
        }
      }
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17'
        }
      }
      steps {
        echo 'test maven app'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      when {
        branch 'main'
      }
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17'
        }
      }
      steps {
        echo 'package maven app'
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.jar'
      }
    }

    stage('Docker BnP') {
      when {
        branch 'main'
      }
      agent {
        docker {
          image 'docker:24.0.7'
          args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
      }
      steps {
        echo 'build and push docker image'
        sh 'docker build -t my-app:latest .'
        // Uncomment and configure the line below if pushing to a registry
        // sh 'docker push my-app:latest'
      }
    }
  }
  tools {
    maven 'Maven 3.6.3'
  }
}
