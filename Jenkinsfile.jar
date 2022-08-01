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
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: "$params.BRANCH", url: "https://github.com/artemovalex/SauseDemo.git"

                // Run Maven on a Unix agent.
               // sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                bat "mvn clean test -DsuiteXmlFile=src/main/resources/regression.xml"
            }

            post {
                success {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
                }
            }
        }
    }
}
