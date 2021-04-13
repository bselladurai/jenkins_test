pipeline {
    agent {
        label 'windows'
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
        jdk "JDK1.8"
    }

    stages {
        stage('PullSCM') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'github', url: 'https://github.com/bselladurai/jenkins_test'
            }
        }
                // Run Maven on a Unix agent.
        stage('Build') {
            steps {
                bat "mvn -Dmaven.test.failure.ignore=true -f discovery-server clean package"
            }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit 'discovery-server/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'discovery-server/target/*.jar'
                }
            }
        }
    }
}
