pipeline {
    agent any
    environment{
	BUILD_NUMBER=${BUILD_NUMBER}
    }
    stages{
         stage('helm-clone'){
             steps{
                 script{
                     git branch: 'test', credentialsId: 'ayushi', url: 'https://github.com/ayushi212001/weather-app.git'
                     //sh "sudo snap remove yq"
                     sh "sudo wget https://github.com/mikefarah/yq/releases/download/v4.27.2/yq_linux_amd64 -O /usr/bin/yq && sudo chown jenkins:jenkins /usr/bin/yq && sudo chmod +x /usr/bin/yq"
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
                           sh "cat values.yaml"
		           sh "yq -i '
				  .a.b[0].c = "cool" |
				  .x.y.z = "foobar" |
				  .person.name = strenv(NAME)
				' file.yaml"
                           sh """
			        yq -i '
				   .authService.tag = strenv(BUILD_NUMBER) |
				   .accountingService.tag = strenv(BUILD_NUMBER)
				' values.yaml
			      """
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
