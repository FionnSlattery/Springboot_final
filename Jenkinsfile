pipeline {
  agent any
  options { skipDefaultCheckout(true) }     // we'll do an explicit git checkout
  tools { jdk 'jdk-17' }                    // Manage Jenkins → Tools → JDK installations → jdk-17

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Chadlikouider/springboot_1.git'
      }
    }

    stage('Build & Package') {
      steps {
        sh 'echo "JAVA_HOME=$JAVA_HOME"'
        sh 'which java && java -version'
        sh 'which mvn || true'
        // Use mvnw if present; otherwise fall back to system Maven
        sh 'if [ -x ./mvnw ]; then chmod +x mvnw && ./mvnw -V -B clean package; else mvn -V -B clean package; fi'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: false
      }
    }

    stage('Deploy') {
      when { branch 'main' }                // only deploy from main
      steps {
        sh 'docker version'
        sh 'docker build -f Dockerfile -t myapp .'
        sh 'docker rm -f myappcontainer || true'
        sh 'docker run --name myappcontainer -p 8081:8080 -d myapp:latest'
      }
    }
  }
}





