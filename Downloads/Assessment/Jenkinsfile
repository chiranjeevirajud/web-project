node(){
    stage('GitClone-SourceCode'){
            sh """
                rm -rf *
            """
            git branch: 'master', credentialsId: 'gitcred-1', url: ''
    }
    stage('Create Docker Image'){
        //sh "docker build -t 502203921648.dkr.ecr.ap-south-1.amazonaws.com/javaapp:${BUILD_NUMBER} ."
        sh "docker build -t ${AWS_ENV_ID}.dkr.ecr.ap-south-1.amazonaws.com/javaapp:${BUILD_NUMBER} ."
        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AWS_ENV_ID}.dkr.ecr.ap-south-1.amazonaws.com"
        sh " docker push ${AWS_ENV_ID}.dkr.ecr.ap-south-1.amazonaws.com/javaapp:${BUILD_NUMBER}"
    }
    node('docker-1'){
        stage('Docker Container Creation'){
            sh """
                docker rm -f $(docker ps -q) || echo 'container is not running'
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AWS_ENV_ID}.dkr.ecr.ap-south-1.amazonaws.com 
                docker run -itd -p 8081:80 --name HelloWorld-App 502203921648.dkr.ecr.ap-south-1.amazonaws.com/javaapp:${BUILD_NUMBER}
            """
        }
    }
}
