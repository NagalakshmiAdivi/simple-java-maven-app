pipeline {
    agent any
 
    // ====== PARAMETERS ======
    parameters {
        choice(
            name: 'Stack',
            choices: ['QA','PROD'],
            description: 'Select environment stack'
        )
    }
 
    // ====== TOOLS ======
    tools {
        jdk 'jdk21'
        maven 'maven-3.9'
    }
 
    environment {
        // Maven and Java paths will be added automatically from tools
        PATH = "${tool 'maven-3.9'}\\bin;${tool 'jdk21'}\\bin;${env.PATH}"
    }
 
    stages {
 
        stage('Checkout') {
            steps {
                echo "Checking out code from configured SCM..."
                checkout scm
            }
        }
 
        stage('Build') {
            steps {
                echo "Building project for stack: ${params.Stack}"
                bat "${tool 'maven-3.9'}\\bin\\mvn clean compile -DStack=${params.Stack}"
            }
        }
 
        stage('Test') {
            steps {
                echo "Running tests..."
                bat "${tool 'Maven-3.9'}\\bin\\mvn test -DStack=${params.Stack}"
            }
        }
 
        stage('Publish Test Reports') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }
 
    post {
        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
 
        success {
            echo "Build succeeded!"
        }
 
        failure {
            echo "Build failed!"
        }
    }
}
