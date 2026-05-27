pipeline {
    agent any

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
            steps {
                sh '''
                cd terraform
                terraform plan
                '''
            }
        }

        stage('Terraform Apply') {
            steps {
                sh '''
                cd terraform
                terraform apply -auto-approve
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh '''
                cd frontend
                docker build -t frontend .
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/
                '''
            }
        }
    }
}
<<<<<<< HEAD:Jenkinsfile
<<<<<<< HEAD:Jenkinsfile
=======
=======
<<<<<<< HEAD:jenkinsfile
=======
<<<<<<< HEAD:Jenkinsfile
=======
>>>>>>> 811b3c8 (fixed Jenkinsfile merge conflict):Jenkinsfile
>>>>>>> 9e839e1 (update):jenkinsfile
pipeline {
    agent any

    stages {

        stage('Build Frontend Image') {
            steps {
                sh 'cd frontend && docker build -t frontend .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
<<<<<<< HEAD:Jenkinsfile
}
>>>>>>> 0cd4102 (terraform infra):jenkinsfile
=======
<<<<<<< HEAD:jenkinsfile
}
=======
}
>>>>>>> 0cd4102 (terraform infra):jenkinsfile
>>>>>>> 811b3c8 (fixed Jenkinsfile merge conflict):Jenkinsfile
>>>>>>> 9e839e1 (update):jenkinsfile
