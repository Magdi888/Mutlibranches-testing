pipeline {
    agent any
    
    stages {
        stage('configuration') {
            steps {
                echo 'BRANCH NAME: ' + env.BRANCH_NAME
                echo sh(returnStdout: true, script: 'env')
            }
        }
        
        stage('Testing') {
            steps {
                script {
                    sh 'echo "Testing"'
                    sh "cat file.txt"
                }
            }
        }
        
        stage("build"){
            when {
                branch 'master'
            }
            
            steps{
                sh 'echo "Build Started"'
            }
        }

        stage("Deploy"){
            when {
                branch 'master'
            }
            
            steps{
                sh 'echo "Deploying App"'
            }
        }
    }
    
    post{
        success{
            setBuildStatus("Build succeeded", "SUCCESS");
        }

        failure {
            setBuildStatus("Build failed", "FAILURE");
        } 
    }
}

void setBuildStatus(String message, String state) {
    step([
        $class: "GitHubCommitStatusSetter",
        reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/Magdi888/Mutlibranches-testing"],
        contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
        errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
        statusResultSource: [$class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", msg: message, state: state]]]
    ]);
}
