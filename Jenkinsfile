pipeline {
  agent any

  environment {
    AWS_REGION   = "ap-south-1"
    CLUSTER_NAME = "simple-eks-cluster"
  }

  stages {

    stage('Checkout') {
      steps {
        git credentialsId: 'github-creds',
            url: 'https://github.com/RahulWakde/eks-terraform-jenkins.git'
      }
    }

    stage('Terraform Init') {
      steps {
        sh 'terraform init'
      }
    }

    stage('Terraform Apply') {
      steps {
        sh 'terraform apply -auto-approve'
      }
    }

    stage('Configure kubectl') {
      steps {
        sh 'aws eks update-kubeconfig --region $AWS_REGION --name $CLUSTER_NAME'
      }
    }

    stage('Deploy NGINX') {
      steps {
        sh 'kubectl apply -f k8s/nginx.yaml'
      }
    }
  }
}

