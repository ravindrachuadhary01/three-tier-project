pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['apply', 'destroy'],
            description: 'Choose Terraform Action'
        )
    }

    stages {

        stage('Terraform Init') {
            steps {
                sh '''
                cd terraform
                terraform init
                '''
            }
        }

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

        stage('Build Frontend Image') {
            when {
                expression { params.ACTION == 'apply' }
            }

            steps {
                sh '''
                cd frontend
                docker build -t ravindrachuadhary01/frontend:v1 .
                docker push ravindrachuadhary01/frontend:v1
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            when {
                expression { params.ACTION == 'apply' }
            }

            steps {
                sh '''
                export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
                kubectl apply -f k8s/
                '''
            }
        }

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
}
