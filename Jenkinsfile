pipeline {
agent 'any'
stages{
    stage("Demo"){
           steps{

               echo 'Hello World'
            }
    }
    stage("Source"){
        parallel{
            stage('CalibrationResults'){
                steps{
                    echo 'Checking out CalibrationResults'
                    checkout([$class: 'GitSCM', branches: [[name: '*/jenkinsfile']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CloneOption', depth: 0, noTags: true, reference: '', shallow: false, timeout: 60], [$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: true, reference: '', timeout: 60, trackingSubmodules: true], [$class: 'RelativeTargetDirectory', relativeTargetDir: 'server-core'],[$class: 'CheckoutOption', timeout: 60]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'ayushi',url: 'https://github.com/ayushi212001/weather-app.git']]])
                }
            }
            stage('Combination'){

                steps{
                    script{
                        echo 'Checking out server spoke'
                        checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CloneOption', depth: 0, noTags: true, reference: '', shallow: false, timeout: 60], [$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: true, reference: '', timeout: 60, trackingSubmodules: true], [$class: 'RelativeTargetDirectory', relativeTargetDir: 'server-spoke'], [$class: 'CheckoutOption', timeout: 60]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'ayushi',url: 'https://github.com/ayushi212001/register.git']]])
                        sh "sudo snap install yq"
                        sh "sudo yq -i values.yaml '.authService.tag.value' 'v2' "
                        sh "sudo yq -i '.accountingService.tag = "v2"' values.yaml"
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
