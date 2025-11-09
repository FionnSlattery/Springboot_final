pipeline {
  agent any
  tools {
    jdk 'jdk-17'           // Use the JDK you just configured
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Chadlikouider/springboot_1.git'
      }
    }

    stage('Build') {
      steps {
        sh 'echo "JAVA_HOME=$JAVA_HOME"'
        sh 'java -version'
        sh 'chmod +x mvnw || true'
        sh './mvnw -V -B clean package'
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: false
      }
    }

    stage('Deploy') {
      steps {
        sh 'docker version'
        sh 'docker build -f Dockerfile -t myapp .'
        sh 'docker rm -f myappcontainer || true'
        sh 'docker run --name myappcontainer -p 8081:8080 -d myapp:latest'
      }
    }
  }
}



