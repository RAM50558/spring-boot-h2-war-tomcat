pipeline {
    agent any
    tools {
        maven 'maven'
        //jdk 'jdk11'
    }
    stages {
        stage("Tools initialization") {
            steps {
                sh "mvn --version"
               // sh "java -version"
            }
        }
        stage("Checkout Code") {
            steps {
                checkout scm
            }
        }
        stage("Check Code Health") {
            when {
                not {
                    anyOf {
                        branch 'master';
                        branch 'devops'
                    }
                }
           }
           steps {
               sh "mvn clean compile"
            }
        }
        stage("Run Test cases") {
            when {
                branch 'master';
            }
           steps {
               sh "mvn clean test"
            }
        }
        stage("Check Code coverage") {
            when {
                branch 'master'
            }
            steps {
               jacoco(
                    execPattern: '**/target/**.exec',
                    classPattern: '**/target/classes',
                    sourcePattern: '**/src',
                    inclusionPattern: 'com/iamvickyav/**',
                    changeBuildStatus: true,
                    minimumInstructionCoverage: '30',
                    maximumInstructionCoverage: '80')
           }
        }
        stage("Build & Deploy Code") {
            when {
                branch 'master'
            }
            steps {
                sh "mvn tomcat7:deploy"
            }
        }
    }
 }
