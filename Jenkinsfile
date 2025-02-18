pipeline {
  agent any
    
  tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/vin2008vin/kube-infra-setup.git']]])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t devopsdocker0009/node-app-1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'devopsdocker0009', variable: 'devopsdocker0009')]) {
                    sh 'docker login -u devopsdocker0009 -p ${devopsdocker0009}'
                 }  
                 sh 'docker push devopsdocker0009/node-app-1.0'
                }
            }
        }
         
     stage('Deploying Node App tohelm chart to Kubernetes') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name kube --region ap-south-1')
          sh "kubectl get ns"
          sh "helm install java ./node-app"
        }
      }
    }

  }
}
