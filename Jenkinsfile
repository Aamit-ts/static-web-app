pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Deploy'){
            steps{
                sshagent(['Nginx_Key']) {
               sh """ 
               echo 'deploying to Nginx Server'
               ssh -o stricthostkeychecking=no ec2-user@13.62.51.228 'sudo rm -rf usr/share/nginx/html/*'
               scp -o stricthostkeychecking=no -r * ec2-user@13.62.51.228:/usr/share/nginx/html/
               echo 'Restarting Nginx Server'
               sleep 20
               ssh -o stricthostkeychecking=no ec2-user@13.62.51.228 'sudo systemctl restart nginx'
               """
             
                    }
                }
        }
        
        Post {
            always{
                echo 'Clening up..'
                CleanWs()
            }
            success{
                echo 'Sending Sucess Notification'
            }
            failure{
                echo 'Sending failure Notification'
            }
        }
    }
}

