pipeline{
    
    agent any 

    parameters{

        choice(name: 'action', choices: 'create\ndestroy', description: 'create/Destroy the infrastructure')
    }

    environment {

        ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        SECRET_KEY = credentials('AWS_SECRET_KEY_ID')
    }
    
    stages{
        
        stage('git checkout'){
            
            steps{

                git branch: 'main', url: 'https://github.com/welkin1004/AWS-Terra-1.git'
            }
        }
        stage('terraform init'){
            
            steps{

                sh 'terraform init'
            }
        }
        stage('terraform plan'){
            
            steps{

                sh """
                 terraform plan -var 'access_key=$ACCESS_KEY' -var 'secret_key=$SECRET_KEY' --var-file=terra.tfvars
                """
            }
        }
        stage('terraform apply'){

            steps{

                script{

                    def apply = false

                    try{
                       
                       input message: 'Can you please confirm the apply', ok: 'Ready to apply terraform'
                       apply = true

                    } catch(err){

                      apply = false
                      currentBuild.result = 'UNSTABLE'
                    }
               
                   if(apply){
                  
                sh """
                 terraform apply -var 'access_key=$ACCESS_KEY' -var 'secret_key=$SECRET_KEY' --var-file=terra.tfvars --auto-approve
                """
                   }

                }
            }
        }
        stage('terraform destroy'){

          steps{
                            sh """
                 terraform destroy -var 'access_key=$ACCESS_KEY' -var 'secret_key=$SECRET_KEY' --var-file=terra.tfvars --auto-approve
                """
          }
        }
    }
}
