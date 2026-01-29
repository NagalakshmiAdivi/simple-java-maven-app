pipeline {
  agent any
  stages {
    stage('build') {
      parallel {
        stage('build') {
          steps {
            echo 'echo \'test\''
          }
        }

        stage('apitest') {
          steps {
            echo 'echo \'apitest\''
          }
        }

      }
    }

  }
}