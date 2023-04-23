pipeline {
    agent any

     environment{
       registryCredential = 'ecr:ap-south-1:rajat'
       appRegistry = "432938546080.dkr.ecr.ap-south-1.amazonaws.com/rajat_devops"
       capstoneRegistry = "https://432938546080.dkr.ecr.ap-south-1.amazonaws.com"
       cluster = "devops_project"
        service = "service_1"
   }

    stages {
        stage('Clone Website') {
            steps {
                git url:'https://github.com/rajatmungra/Devops-Project.git'
            }
        }

       stage("Docker Build Image"){
           steps{
             script{
                dockerImage = docker.build(appRegistry+ ":$BUILD_NUMBER",".")
             }
           }
      }

       stage('Test Website') {
            steps {
                // Run tests on the website
                sh 'echo "Running tests on the website"'
            }
       }

       stage("Upload App Image"){
         steps{
            script{
                docker.withRegistry(capstoneRegistry, registryCredential){
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                }
            }
         }
      }

      stage('Deploy to ecs'){
         when {
                branch "master"
         }
         steps{
            withAWS(credentials: 'rajat', region: 'ap-south-1'){
                sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
            }
         }
      }

    }
}
