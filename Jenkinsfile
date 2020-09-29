pipeline{
        agent { label 'slave' }
        stages{
          stage('Build Image'){
            steps{
                sh 'docker build -f Dockerfile  . -t genedy377/booster_ci_cd_project:v1.0'
            }
         }
          stage('Push Image'){
            steps{

                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]) {
                    sh 'docker login --username $USERNAME --password $PASSWORD'
                    sh 'docker push genedy377/booster_ci_cd_project:v1.0'
                }

            }
         }  
          stage('Deploy'){
            steps{

                sh 'docker run -d  -p 8000:3000  genedy377/booster_ci_cd_project:v1.0'

            }
          }
        }
        post
        {
          success{
                    slackSend (color: '#00FF00', message: "SUCCESSES: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}console)") 
                 }
          failure{
                    slackSend (color: '#E83009', message: "FAILURE: Job '${env.JOB_NAME} ${env.GIT_COMMIT} ${env.GITBRANCH} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}/console)") 
                 }
          aborted{
                    slackSend (color: '#E8E209', message: "SUCCESSES: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})console") 
                    
                 }
         
        }
}
