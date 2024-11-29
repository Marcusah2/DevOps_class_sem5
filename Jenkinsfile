pipeline {
    agent any

    environment {
        TF_VERSION = "1.5.3"  // Set the desired Terraform version
        AWS_REGION = "us-east-1"  // Specify your region (change as needed)
        TERRAFORM_DIR = "/path/to/your/terraform/code"  // Path to your Terraform configuration files
    }

    stages {
        stage('Prepare Environment') {
            steps {
                script {
                    // Check if terraform is already installed
                    if (!sh(script: "command -v terraform", returnStatus: true)) {
                        echo "Terraform is not installed. Installing Terraform version ${TF_VERSION}..."

                        // Download the specified version of Terraform
                        sh '''
                        curl -LO https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip

                        # If terraform binary exists, remove it first
                        if [ -f /usr/local/bin/terraform ]; then
                            sudo rm /usr/local/bin/terraform
                        fi

                        # Unzip the downloaded file and overwrite any existing binary
                        unzip -o terraform_${TF_VERSION}_linux_amd64.zip

                        # Move the terraform binary to /usr/local/bin/ if needed
                        sudo mv terraform /usr/local/bin/
                        '''
                    } else {
                        echo "Terraform is already installed."
                    }
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    // Initialize Terraform working directory
                    sh """
                    cd ${TERRAFORM_DIR}
                    terraform init -backend-config="region=${AWS_REGION}" -backend-config="bucket=my-terraform-state"
                    """
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Run Terraform plan
                    sh """
                    cd ${TERRAFORM_DIR}
                    terraform plan -out=tfplan
                    """
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    // Apply Terraform changes
                    sh """
                    cd ${TERRAFORM_DIR}
                    terraform apply -auto-approve tfplan
                    """
                }
            }
        }

        stage('Verification') {
            steps {
                script {
                    // Optionally run verification steps or output Terraform state
                    sh """
                    cd ${TERRAFORM_DIR}
                    terraform show
                    """
                }
            }
        }
    }

    post {
        always {
            // Clean up or notify stakeholders
            echo 'Pipeline completed.'
        }
    }
}
