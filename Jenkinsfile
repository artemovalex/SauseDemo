pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    parameters {
        gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', description: 'Select branch to run tests', name: 'BRANCH', type: 'PT_BRANCH'
    }

    stages {
   //     stage('test') {
   //         steps {
   //             bat(returnStdout: true, script: "echo \"Buils starting...\"").trim()
   //         }
   //     }
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: "${params.BRANCH}", url: 'https://github.com/artemovalex/SauseDemo.git'

                // Run Maven on a Unix agent.
                // sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
            //    bat "ls"
              //  bat "mvn clean test -DsuiteXmlFile=src/main/resources/regression.xml"
              //  bat "chcp 65001\n mvn clean test -DsuiteXmlFile=src/main/resources/regression.xml"
                bat (script: "mvn clean test -DsuiteXmlFile=src/main/resources/regression.xml",  returnStatus: true)
            }
            //chcp 65001\n
        }
         stage('Allure') {
             steps {
                 script {
                     allure([
                         includeProperties: false,
                         jdk: '',
                         properties: [],
                         reportBuildPolicy: 'ALWAYS',
                         results: [[path: 'target/allure-results']]
                     ])
                 }
             }
         }
    }
}