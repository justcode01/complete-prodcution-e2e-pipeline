@Library('jenkins_shared_lib') _

pipeline{
    agent any

    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'choose create/Destroy')
        string(name: 'ImageName', description: 'name of the docker build', defaultValue: 'javaapp')
        string(name: 'ImageTag', description: 'tag of the docker build', defaultValue: 'v1')
        string(name: 'DockerHubUser', description: 'name of the app', defaultValue: 'mayank0106')

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

        stage('Maven Build: Maven'){
        when { expression { params.action == 'create' }}    
            steps{
                script{
                    mvnBuild()
                }
            }
        }

        stage('Docker Image Build'){
        when { expression { params.action == 'create' }}    
            steps{
                script{
                    dockerBuild("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                }
            }
        }

        // stage('Docker image scan: Trivy'){
        // when { expression { params.action == 'create' }}    
        //     steps{
        //         script{
        //           dockerImageScan("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")  
        //         }
        //     }
        // }

        stage('Docker Image Push: Dockerhub'){
        when { expression { params.action == 'create' }}    
            steps{
                script{

                    dockerImagePush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                }
            }
        }
    }   
}

