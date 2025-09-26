pipeline {
    agent any
    environment {
        TERRAFORM_HOME = tool name: 'terraform'
        PATH = "${TERRAFORM_HOME}:${env.PATH}"
    }
    parameters {
        choice(name: 'TERRAFORM_ACTION', choices: ['plan', 'apply', 'destroy'], description: 'Terraform action to perform')
        string(name: 'BRANCH', defaultValue: 'master', description: 'The branch to build and deploy')
        // Add other parameters as needed, e.g., for different environments or variable files
        // string(name: 'TF_WORKSPACE', defaultValue: 'default', description: 'Terraform workspace to use')
    }

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
                    echo "gsgsgsgsdgd: ${BRANCH}"
                    echo "sdgsdgfdsjkbksjdfgjfdsgjkgjkfdjk : ${GIT_COMMIT}"
    //                sh 'sleep 60'
  //                  sh 'which terraform'
                    // Ensure Terraform is available in the PATH
                    // If not, you might need to use a Docker agent with Terraform,
                    // or use the Terraform Jenkins plugin, or install it manually.
                    sh 'terraform version' // Verify terraform installation
                    sh 'terraform init'
                    // If using workspaces:
                    // sh "terraform workspace select ${params.TF_WORKSPACE} || terraform workspace new ${params.TF_WORKSPACE}"
                }
            }
        }
        stage('Terraform Plan') {
            when {
                expression { params.TERRAFORM_ACTION == 'plan' || params.TERRAFORM_ACTION == 'apply' }
            }
            steps {
                script {
                    if (params.TERRAFORM_ACTION == 'apply') {
                        sh 'terraform plan -out=tfplan -input=false'
                        // Optional: Archive the plan file
                        // archiveArtifacts artifacts: 'tfplan', fingerprint: true
                    } else {
                        sh 'terraform plan -input=false'
                    }
                }
            }
        }

        stage('Terraform Apply / Destroy') {
            when {
                expression { params.TERRAFORM_ACTION == 'apply' || params.TERRAFORM_ACTION == 'destroy' }
            }
            steps {
                script {
                    if (params.TERRAFORM_ACTION == 'apply') {
                        // Optional: Add a manual approval step before applying
                        // input message: "Proceed with Terraform Apply?", ok: "Apply"
                        sh 'terraform apply -auto-approve -input=false tfplan' // Apply the saved plan
                        // Or if not using a saved plan:
                        // sh 'terraform apply -auto-approve -input=false'
                    } else if (params.TERRAFORM_ACTION == 'destroy') {
                        // Optional: Add a manual approval step before destroying
                        // input message: "Proceed with Terraform Destroy?", ok: "Destroy"
                        sh 'terraform destroy -auto-approve -input=false'
                    }
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
