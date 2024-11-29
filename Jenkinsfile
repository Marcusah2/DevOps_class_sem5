pipeline {
    agent any

    environment {
        // Set Terraform version
        TF_VERSION = "1.5.3"  // Set the desired Terraform version
        AWS_REGION = "us-east-1"  // Specify your region (change as needed)
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout code from GitHub
                checkout scm
            }
        }

        stage('Install Terraform') {
            steps {
                script {
                    // Install Terraform (if not already installed)
                    sh '''
                    if ! command -v terraform &>/dev/null; then
                        echo "Terraform not found. Installing..."
                        curl -LO https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip
                        unzip terraform_${TF_VERSION}_linux_amd64.zip
                        sudo mv terraform /usr/local/bin/
                    fi
                    '''
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    // Initialize Terraform working directory
                    sh '''
                    terraform init -backend-config="region=${AWS_REGION}" -backend-config="bucket=my-terraform-state"
                    '''
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Run Terraform plan
                    sh '''
                    terraform plan -out=tfplan
                    '''
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    // Apply Terraform changes
                    sh '''
                    terraform apply -auto-approve tfplan
                    '''
                }
            }
        }

        stage('Verification') {
            steps {
                script {
                    // Optionally run verification steps or output Terraform state
                    sh '''
                    terraform show
                    '''
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
