pipeline {
    agent any

    stages {
            stage('clone repo') {
                steps {
                    git url: 'https://github.com/Spanwars/duotask.git', branch:'master'
                }
            }
            stage('Pre build Cleanup'){
                steps{
                    sh 'docker system prune -f'
                }
            }
            stage('Build') {
                steps {
                    sh ''' 
                    docker network create duo-net
                    docker build -t duo-task:v1 .
                    docker run -d --network duo-net --name duo-task duo-task:v1
                    docker run -d -p 80:80 --mount type=bind,source=$(pwd)/nginx.conf,target=/etc/nginx/nginx.conf --network duo-net --name nginx nginx:alpine
                    '''
                }
            }   
    }
    
}
