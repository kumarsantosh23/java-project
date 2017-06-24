pipeline {
  agent none  

  stages {
    stage('Unit Tests') {
       agent {
        node ('master')
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build') {
      agent {
        node ('master')
      }
      steps {
        sh 'ant -f build.xml -v'
      }
       post {
         success {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
 }

    stage('deploy') {
         agent {
        node ('master')
      }
      steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
    }
  }
 
   stage('running on another node') {
    agent {
     label 'slave1'
   }
    steps {
       sh "wget http://santu23551.mylabserver.com/rectagles/all/rectangle_${env.BUILD_NUMBER}.jar"
       sh "java -jar rectangle_${env.BUILD_NUMBER}.jar  3 4"
  } 
 }
 }
}
