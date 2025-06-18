pipeline {
    agent any
    environment {
        TERRAFORM_HOME = tool name: 'terraform'
        PATH = "${TERRAFORM_HOME}:${env.PATH}"
    }
//    parameters {
//        choice(name: 'TERRAFORM_ACTION', choices: ['plan', 'apply', 'destroy'], description: 'Terraform action to perform')
//        // Add other parameters as needed, e.g., for different environments or variable files
 //       // string(name: 'TF_WORKSPACE', defaultValue: 'default', description: 'Terraform workspace to use')
//    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    sh 'printenv | grep -i terraform'
                    sh 'sleep 60'
  //                  sh 'which terraform'
                    // Ensure Terraform is available in the PATH
                    // If not, you might need to use a Docker agent with Terraform,
                    // or use the Terraform Jenkins plugin, or install it manually.
                    sh 'terraform version' // Verify terraform installation
            //        sh 'terraform init'
                    // If using workspaces:
                    // sh "terraform workspace select ${params.TF_WORKSPACE} || terraform workspace new ${params.TF_WORKSPACE}"
                }
            }
        }
    }
    post {
        always {
            echo 'Terraform job finished.'
            // Clean up workspace, tfplan, etc. if needed
            // deleteDir()
        }
        success {
            echo 'Terraform execution successful.'
        }
        failure {
            echo 'Terraform execution failed.'
        }
    }
}
