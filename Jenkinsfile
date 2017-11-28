// Syntax-Reference: https://github.com/jenkinsci/pipeline-model-definition-plugin/wiki/Syntax-Reference
//                   https://jenkins.io/doc/book/pipeline/syntax/
// CheatSheet: https://www.cloudbees.com/sites/default/files/declarative-pipeline-refcard.pdf
pipeline {
    agent {
        label 'QUE-CPL-python27_x64'
    }

    options {
        ansiColor('xterm')
    }

    parameters {
        // Parameters are available as ENV variable
        booleanParam(defaultValue: false, description: 'Check this box the create a release with your current branch', name: 'release')
    }
    environment {
        IS_RELEASE = "${params.release}"
        SERVER_CRED = credentials('471c6fac-26ab-4e56-9223-6852d30ef89f')
        SERVER_ADDRESS = 'http://10.101.211.54:3141'
    }
    stages {
        stage('Run tests') {
            steps {
                script {
                    echo "N/A"
                }
            }
        }

        stage('Create release') {
            when {
                environment name: "IS_RELEASE", value: "true"
            }
            steps {
                script {
                    bat "pip install devpi-client wheel && devpi.exe use ${env.SERVER_ADDRESS} && devpi.exe login ${env.SERVER_CRED_USR} --password ${env.SERVER_CRED_PSW} && devpi.exe use /${env.SERVER_CRED_USR}/release && devpi.exe upload --formats bdist_wheel"

                    // echo pip install -i "buildmachine/release" --extra-index-url "%server_address%" [PACKAGE_NAME]
                }
            }
        }
    }
    post {
        always {
            echo 'This will always run'
            // Publish test results
            // junit '**/test_result.xml'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
        }
    }
}