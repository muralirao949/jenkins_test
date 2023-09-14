properties([pipelineTriggers([githubPush()])])
pipeline {
    agent any
    
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
    }

    stages {
        stage("print0") {
            steps {
                echo "printing 1"
            }
        }
        stage("print2") {
            steps {
                echo "printing 1"
            }
        }
        stage('Build') { 
            agent {
                label 'linux'
            }
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'git', url: 'git@github.com:muralirao949/jenkins_test.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit 'api-gateway/target/surefire-reports/*.xml'
                    archiveArtifacts 'api-gateway/target/*.jar'
                    }
            }
        }
stage('Approval') {
    options{
        timeout(time:1, unit: 'MINUTES')
    }
  steps {
emailext body: "Please click at $BUILD_URL/input to approve the deployment /n This link is valid for 1 minute", to: "muralirao949@gmail.com", subject: '$PROJECT_NAME is ready for deployment - Build number is $BUILD_NUMBER - Please approve to proceed with Deployment'
                    
input "Please approve to proceed with deployment"
  }
}
stage('Deployment'){
    steps {
        echo "Deploying application"
    }
}
    }
}
