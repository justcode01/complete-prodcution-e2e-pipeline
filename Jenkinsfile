@Library('jenkins_shared_lib') _

pipeline{
    agent any

    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'choose create/Destroy')

    }
    
    stages{

        stage('Git Checkout'){
        when { expression { params.action == 'create' }}    
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/justcode01/complete-prodcution-e2e-pipeline.git"
            )
            }
       }

       stage('Unit test maven'){
        when { expression { params.action == 'create' }}
            steps{
                script{
                    mvnTest()
                }
                
            }
        }
    }   
}

