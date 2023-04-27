/*
Pipeline jenkins for application java with 4 stages
Git - recupere le repo git 
Compile - compile l'application mvn clean compile
Test  mvn test et recuperer les logs des test junit dans tout les cas '/target/surefire-reports/TEST-*.xml'
Package mvn -DskipTests -Dmaven.test.skip package et si success archiveArtifacts 'target/*.jar'
*/

pipeline {
    agent any
    parameters {
        string(name: 'URL_MOVIE', defaultValue: 'https://github.com/matthcol/movieapp_jdbc.git', description: 'Link of url ')
        string(name: 'BRANCH_GIT', defaultValue: 'main', description: 'branch of github link')
    }
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                 git branch:echo "${params.BRANCH_GIT}",
                     url:echo "${params.URL_MOVIE}"
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
                     archiveArtifacts 'target/*.jar, target/*.war'
                }

            }
        }
    }
}