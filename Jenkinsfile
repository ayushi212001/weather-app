pipeline {
    agent any
    stages{
        stage('helm-update'){
            parallel{
                stage('helm-clone'){
                    steps{
                        script{
                            git branch: 'test', credentialsId: 'ayushi', url: 'https://github.com/ayushi212001/weather-app.git'
                            sh "sudo snap install yq"

                        }
                    }
                }
                stage('update-values'){
                    steps{
                        script{
                            dir('biz-book-helm'){
                              sh "pwd"  
                              git branch: 'main', credentialsId: 'ayushi', url: 'https://github.com/ayushi212001/register.git'

                              sh "sudo yq -i '.authService.tag = "v2"' public/values.yaml"
                              sh "sudo yq -i '.accountingService.tag = "v2"' public/values.yaml"
                              sh "sudo git add ."  
                              sh "sudo git commit -m 'updated values'"
                              withCredentials([usernamePassword(credentialsId: 'ayushi', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                                sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/ayushi212001/register.git')
                              } 
                            }  


                        }
                    }
                }
                
                          
            }
        }    
    }
}    
