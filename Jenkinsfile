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
        string(name: 'URL_MOVIE', defaultValue: '', description: 'Link of url ')
        string(name: 'BRANCH_GIT', defaultValue: '', description: 'branch of github link')
        choice(name: 'CHOICE', choices: ['JDK11', 'JDK17'], description: 'JDK version of compiler  - build')
    }
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
        jdk "${params.CHOICE}"
    }

    stages {
        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                 git branch:"${params.BRANCH_GIT}",
                     url:"${params.URL_MOVIE}"
            }

            /*post {
                success {
                    script {
                        if (params.CHOICE == 'JDK11'){
                                sh "sed -i s@<maven.compiler.target>.*</maven.compiler.target>@<maven.compiler.target>11</maven.compiler.target>@ pom.xml"
                                sh "sed -i s@<maven.compiler.source>.*</maven.compiler.source>@<maven.compiler.source>11<maven.compiler.source>@ pom.xml"
                        }
                        else if (params.CHOICE == 'JDK17'){
                                sh "sed -i s@<maven.compiler.target>.*</maven.compiler.target>@<maven.compiler.target>17</maven.compiler.target>@ pom.xml"
                                sh "sed -i s@<maven.compiler.source>.*</maven.compiler.source>@<maven.compiler.source>17<maven.compiler.source>@ pom.xml"
                        }
                        else {

                        }

                    }
                    
                }

            }*/

        }
        stage('Config') {
            steps {
                script {
                    if("${params.CHOICE}"=='JDK11')
                    {
                        sh "sed -i s@<maven.compiler.target>.*</maven.compiler.target>@<maven.compiler.target>11</maven.compiler.target>@ pom.xml"
                        sh "sed -i s@<maven.compiler.source>.*</maven.compiler.source>@<maven.compiler.source>11<maven.compiler.source>@ pom.xml"
                    }
                    if("${params.CHOICE}"=='JDK17'){
                        sh "sed -i s@<maven.compiler.target>.*</maven.compiler.target>@<maven.compiler.target>17</maven.compiler.target>@ pom.xml"
                        sh "sed -i s@<maven.compiler.source>.*</maven.compiler.source>@<maven.compiler.source>17<maven.compiler.source>@ pom.xml"
                    }
                }
               
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