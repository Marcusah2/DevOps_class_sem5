pipeline {
    agent any

    environment {
        TF_VERSION = "1.5.3"  // Set the desired Terraform version
        AWS_REGION = "us-east-1"  // Specify your region (change as needed)
        GITHUB_REPO = "https://github.com/Marcusah2/DevOps_class_sem5.git"  // GitHub repository URL
    }

    stages {
        stage('Checkout Terraform Code from GitHub') {
            steps {
                script {
                    echo "Cloning Terraform code from GitHub repository..."

                    // Clone the repository from GitHub into the workspace
                    try {
                        git url: "${GITHUB_REPO}", branch: 'main'  // Adjust the branch name if needed
                        echo "Git repository cloned successfully."
                    } catch (Exception e) {
                        echo "Git clone failed: ${e.getMessage()}"
                        error "Failed to clone Git repository"
                    }

                    // Verify the directory structure
                    echo "Listing files in the workspace:"
                    sh "ls -la ${WORKSPACE}"
                }
            }
        }

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

                        // Ensure terraform is available on the system path
                        echo "Adding Terraform to PATH..."
                        sh 'export PATH=$PATH:/usr/local/bin'
                    } else {
                        echo "Terraform is already installed."
                    }
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    // Ensure the terraform binary is accessible
                    echo "Verifying Terraform installation..."
                    sh "terraform --version" // This will confirm terraform is available in the PATH

                    // Initialize Terraform working directory
                    echo "Running terraform init..."
                    try {
                        // Ensure to 'cd' into the directory after git checkout
                        sh """
                        cd ${WORKSPACE}  // This is where the GitHub repo will be cloned
                        terraform init -backend-config="region=${AWS_REGION}" -backend-config="bucket=my-terraform-state"
                        """
                    } catch (Exception e) {
                        echo "Terraform init failed: ${e.getMessage()}"
                        error "Failed to initialize Terraform"
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Run Terraform plan
                    echo "Running terraform plan..."
                    try {
                        sh """
                        cd ${WORKSPACE}  // This is where the GitHub repo will be cloned
                        terraform plan -out=tfplan
                        """
                    } catch (Exception e) {
                        echo "Terraform plan failed: ${e.getMessage()}"
                        error "Failed to create Terraform plan"
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    // Apply Terraform changes
                    echo "Running terraform apply..."
                    try {
                        sh """
                        cd ${WORKSPACE}  // This is where the GitHub repo will be cloned
                        terraform apply -auto-approve tfplan
                        """
                    } catch (Exception e) {
                        echo "Terraform apply failed: ${e.getMessage()}"
                        error "Failed to apply Terraform changes"
                    }
                }
            }
        }

        stage('Verification') {
            steps {
                script {
                    // Optionally run verification steps or output Terraform state
                    echo "Verifying Terraform state..."
                    sh """
                    cd ${WORKSPACE}  // This is where the GitHub repo will be cloned
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
