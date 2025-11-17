pipeline{
    agent any
    
    environment {
        ARTIFACT_DIR = "/opt/aspnetapp/release"
        DEPLOY_DIR = "/opt/aspnetapp/deploy"
       
        
    }
    parameters{
         string(name: 'ARTIFACT_ID', defaultValue: '14', description: 'Select the deployment artifact')
    }
  
    stages{
        stage('Build'){
          when {
            branch 'develop'
          }
            steps {
                sh 'dotnet restore'
                sh 'dotnet publish -o ${ARTIFACT_DIR}/${BUILD_NUMBER}'
            }
        }

      stage('Deploy'){
            when {
              branch 'master'
            }
            steps{
               sh """
                #!/bin/bash 
                exist=\$(ls ${ARTIFACT_DIR} | grep -o  ${ARTIFACT_ID} && echo 0 || echo 1)

                if [[ \$exist == 1 ]]; then
                  echo "invalid artifact ID"
                  return
                fi

                ln -sf ${ARTIFACT_DIR}/${ARTIFACT_ID}/* ${DEPLOY_DIR}
                
                  """
                }
          }

      stage("post"){
        steps{
          sh 'sudo systemctl restart dotnet-hello-app'
            }
        }
        
        
    }
post{
    success {
            mail bcc: '', 
                 body: "The Jenkins build ${env.JOB_NAME} #${env.BUILD_NUMBER} was successful. View details: ${env.BUILD_URL}", 
                 cc: '', 
                 from: '', 
                 subject: "[SUCCESS] Jenkins Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}", 
                 to: 'adarshkumar4ce@gmail.com'
        }

    failure {
            mail bcc: '', 
                 body: "The Jenkins build ${env.JOB_NAME} #${env.BUILD_NUMBER} FAILED. Check console log: ${env.BUILD_URL}", 
                 cc: '', 
                 from: '', 
                 subject: "[FAILED] Jenkins Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}", 
                 to: 'adarshkumar4ce@gmail.com'
        }

    }
}
