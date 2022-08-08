pipeline {
    agent any
    stages{
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
                     sh "pwd"
                     dir('biz-book-helm'){
                        git branch: 'main', credentialsId: 'ayushi', url: 'https://github.com/ayushi212001/register.git'
                        sh "pwd"
                        dir('public'){
                           sh "pwd" 
                           sh "ls"
                           sh "sudo yq -i $WORKSPACE/public/values.yaml '.authService.tag.value' 'v2' "
                           sh "sudo yq -i '.accountingService.tag = v2' values.yaml"
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
