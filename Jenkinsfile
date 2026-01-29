node {
 
    // ====== PARAMETERS ======
    properties([
        parameters([
            choice(
                name: 'Stack',
                choices: ['QA', 'UAT', 'PROD'],
                description: 'Select environment stack'
            )
        ])
    ])
 
    // ====== TOOLS ======
    def mvnHome = tool 'maven-3.9'
    def javaHome = tool 'jdk21'
 
    // ====== ENV VARIABLES ======
    env.PATH = "${mvnHome}/bin:${javaHome}/bin:${env.PATH}"
 
    try {
 
        stage('Checkout') {
            echo "Checking out source code..."
            checkout scm
        }
 
        stage('Build') {
            echo "Building project for stack: ${params.Stack}"
            bat "mvn clean compile -DStack=${params.Stack}"
        }
 
        stage('Test') {
            echo "Running tests..."
            bat "mvn test -DStack=${params.Stack}"
        }
 
        stage('Publish Test Reports') {
            junit '**/target/surefire-reports/*.xml'
        }
 
    } catch (err) {
 
        currentBuild.result = 'FAILURE'
        echo "Build failed: ${err}"
        throw err
 
    } finally {
 
        stage('Cleanup') {
            echo "Cleaning workspace..."
            cleanWs()
        }
    }
}
 
