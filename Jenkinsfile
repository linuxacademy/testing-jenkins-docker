pipeline {
  agent any
  environment {
    MAJOR_VERSION = 1
    JENKINS_IP = '52.31.133.94'
  }
  stages {
    stage('Unit Tests') {
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
    stage('Test on CentOS') {
      agent {
        docker {image 'fabric8/java-centos-openjdk8-jdk:1.4.0'}
      }
      steps {
        sh "curl ${env.JENKINS_IP}/rectangles/all/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar -o rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    stage('Test on Debian') {
      agent {
        docker {image 'openjdk:12'}
      }
      steps {
        sh "curl ${env.JENKINS_IP}/rectangles/all/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar -o rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    } 
  }
}
