pipeline {
    agent any

    environment {
        TF_VERSION = "1.5.3"                 // Desired Terraform version
        AWS_REGION = "us-east-1"            // AWS Region
        GITHUB_REPO = "https://github.com/Marcusah2/DevOps_class_sem5.git" // GitHub repo URL
        AWS_CREDENTIALS_ID = "aws-id" // Replace with the ID of the AWS credentials in Jenkins
    }

    stages {
        stage('Checkout Terraform Code from GitHub') {
            steps {
                script {
                    echo "Cloning Terraform code from GitHub repository..."

                    // Clone the repository from GitHub into the workspace
                    git url: "${GITHUB_REPO}", branch: 'main'

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
                    def terraformExists = sh(script: "command -v terraform", returnStatus: true) == 0

                    if (!terraformExists) {
                        echo "Terraform is not installed. Installing Terraform version ${TF_VERSION}..."

                        sh '''
                        # Download Terraform
                        curl -LO https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip

                        # Unzip Terraform
                        unzip terraform_${TF_VERSION}_linux_amd64.zip

                        # Move to /usr/local/bin
                        sudo mv terraform /usr/local/bin/
                        '''

                        // Verify Terraform installation
                        sh "terraform --version"
                    } else {
                        echo "Terraform is already installed."
                    }
                }
            }
        }

        stage('Terraform Init') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: "${AWS_CREDENTIALS_ID}"
                ]]) {
                    script {
                        echo "Initializing Terraform..."
                        sh '''
                        export AWS_REGION="${AWS_REGION}"
                        cd ${WORKSPACE}
                        terraform init \
                            -backend-config="bucket=my-terraform-state-bucket" \
                            -backend-config="region=${AWS_REGION}" \
                            -backend-config="key=terraform/state"
                        '''
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: "${AWS_CREDENTIALS_ID}"
                ]]) {
                    script {
                        echo "Creating Terraform plan..."
                        sh '''
                        export AWS_REGION="${AWS_REGION}"
                        cd ${WORKSPACE}
                        terraform plan -out=tfplan
                        '''
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: "${AWS_CREDENTIALS_ID}"
                ]]) {
                    script {
                        echo "Applying Terraform changes..."
                        sh '''
                        export AWS_REGION="${AWS_REGION}"
                        cd ${WORKSPACE}
                        terraform apply -auto-approve tfplan
                        '''
                    }
                }
            }
        }

        stage('Verification') {
            steps {
                script {
                    echo "Verifying Terraform state..."
                    sh '''
                    cd ${WORKSPACE}
                    terraform show
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Terraform pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
