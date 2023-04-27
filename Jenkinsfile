/*
Pipeline jenkins for application java with 4 stages
Git - recupere le repo git 
Compile - compile l'application mvn clean compile
Test  mvn test et recuperer les logs des test junit dans tout les cas '/target/surefire-reports/TEST-*.xml'
Package mvn -DskipTests -Dmaven.test.skip package et si success archiveArtifacts 'target/*.jar'
*/

pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/jglick/simple-maven-project-with-tests.git'
            }
        }
        stage('Compile') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean compile"

                // To run Maven on a Windows agent, use  // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                sh "mvn -DskipTests -Dmaven.test.skip package"
            }
            post {
                success {
                     archiveArtifacts 'target/*.jar'
                }

            }
        }
    }
}