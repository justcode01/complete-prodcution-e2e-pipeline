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

        stage('Integration Testing'){
        when { expression { params.action == 'create' }}
            steps{
                script{
                    mvnIntegrationTest()
                }
            }
        }

        stage('Static code Analysis; Sonarqube'){
        when { expression { params.action == 'create' }}    
            steps{
                script{
                    def SonarQubecredentialsId = 'sonar-server'
                    staticCodeAnalysis(SonarQubecredentialsId)
                }
            }
        }

        stage('Quality gate status check: Sonarqube'){
        when { expression { params.action == 'create' }}    
            steps{
                script{
                    def SonarQubecredentialsId = 'sonar-server'
                    QualityGateStatus(SonarQubecredentialsId)
                }
            }
        }
    }   
}

