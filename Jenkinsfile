pipeline {
    agent any

    tools {
        // Install the Maven version configured as "Maven" and add it to the path.
        maven "Maven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/SiyaAmonkar/persistentWebApp.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.war'
                }
            }
        }
        stage('Deploy')
        {   steps{
            deploy adapters: [tomcat8(credentialsId: '70028b46-d724-4557-b00e-111dd5eac392', path: '', url: 'http://ec2-18-208-192-104.compute-1.amazonaws.com:9090')], contextPath: 'Persistent', war: 'target/PersistentWebApp.war'
        }
        }
    }
}
