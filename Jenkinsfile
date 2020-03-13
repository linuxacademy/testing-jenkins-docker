pipeline {
  agent any
  environment {
    MAJOR_VERSION = 1
  }
  stages {
    stage('Unit Tests - Debian') {
      agent {
        docker { image 'openjdk:8u121-jre' }
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    stage('Unit Tests - CentOS') {
      agent {
        docker { image 'fabric8/java-centos-openjdk8-jdk:1.4.0' }
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build') {
      steps {
        sh 'ant -f build.xml -v'
      }
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
    }
    stage('publish') {
      steps {
        sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
      }
    }
  }
}
