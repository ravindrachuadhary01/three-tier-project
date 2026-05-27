pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['apply', 'destroy'],
            description: 'Terraform Action'
        )
    }

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        DOCKER_IMAGE = "ravindrachuadhary01/frontend:v1"
    }

    stages {

        /* ---------------- AWS CHECK ---------------- */
        stage('AWS Identity Check') {
            steps {
                sh '''
                aws --version
                aws sts get-caller-identity
                '''
            }
        }

        /* ---------------- TERRAFORM INIT ---------------- */
        stage('Terraform Init') {
            steps {
                sh '''
                cd terraform
                terraform init
                '''
            }
        }

        /* ---------------- TERRAFORM PLAN ---------------- */
        stage('Terraform Plan') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                sh '''
                cd terraform
                terraform plan
                '''
            }
        }

        /* ---------------- TERRAFORM APPLY ---------------- */
        stage('Terraform Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                sh '''
                cd terraform
                terraform apply -auto-approve
                '''
            }
        }

        /* ---------------- DOCKER BUILD ---------------- */
        stage('Docker Build') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                sh '''
                cd frontend
                docker build -t $DOCKER_IMAGE .
                '''
            }
        }

        /* ---------------- DOCKER PUSH ---------------- */
        stage('Docker Push') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                sh '''
                docker push $DOCKER_IMAGE
                '''
            }
        }

        /* ---------------- KUBERNETES DEPLOY ---------------- */
        stage('Deploy to Kubernetes') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                sh '''
                export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
                kubectl apply -f k8s/
                kubectl get pods
                '''
            }
        }

        /* ---------------- DESTROY APPROVAL ---------------- */
        stage('Destroy Approval') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                input message: "⚠ Are you sure you want to DESTROY infrastructure?"
            }
        }

        /* ---------------- TERRAFORM DESTROY ---------------- */
        stage('Terraform Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                sh '''
                cd terraform
                terraform destroy -auto-approve
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully 🚀"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}