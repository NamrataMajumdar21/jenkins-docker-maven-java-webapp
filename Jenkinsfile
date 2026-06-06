/*pipeline {
    agent {
        label "linuxbuildnode"
        }

stages {
    stage ('SCM') {
        steps {
            git 'https://github.com/NamrataMajumdar21/jenkins-docker-maven-java-webapp.git'
            }
        }
    stage ('Build by Maven Package') {
        steps {
            sh 'mvn clean package'       
            }
        }
    stage ('Build Docker OWN image') {
        steps {
            sh "docker build -t  namrata21101998/javaweb:${BUILD_TAG} ."
            }
        }
    stage ('Push image to Docker HUB') {
        steps {
            withCredentials([string(credentialsId: 'DOCKER_HUB_PWD', variable: 'DOCKER_HUB_PASS_CODE')]) {
               sh "sudo docker login -u  namrata21101998 -p $DOCKER_HUB_PASS_CODE"
                sh "sudo docker push  namrata21101998/javaweb:${BUILD_TAG}" 
                }
            }
        }
    stage ('Deploy webApp in DEV env') {
        steps {
            sh 'sudo docker rm -f myjavaapp'
            sh "sudo docker run -d -p 8080:8080 --name myjavaapp namrata21101998/javaweb:${BUILD_TAG}"
            }
        }
    stage ('Deploy webApp in QA Test env') {
        steps {
            sshagent(['QA_ENV_SSH_CRED']) {
                sh "ssh -o StrictHostKeyChecking=no ec2-user@13.232.108.218 sudo docker rm -f myjavaapp"
                sh "ssh ec2-user@13.232.108.218 sudo docker run -d -p 8080:8080 --name myjavaapp namrata21101998/javaweb:${BUILD_TAG}"
                    }
                }
            }
    stage ('QAT Testing') {
        steps {
            retry(10) {
                sh 'curl --silent http://13.232.108.218:8080/java-web-app/ | grep india'
                    }
                }
            }
    stage('approved') {
            steps {
            script {
                Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                echo 'userInput: ' + userInput

                if(userInput == true) {
                    // do action
                } else {
                    // not do action
                    echo "Action was aborted."
                        }
                
                }
                }
            }
    stage('Deploy webAPP in Prod Env') {
            steps {
               sshagent(['QA_ENV_SSH_CRED']) {
                sh "ssh -o StrictHostKeyChecking=no ec2-user@3.108.52.98 sudo docker rm -f myjavaapp"
                sh "ssh ec2-user@3.108.52.98 sudo docker run -d -p 8080:8080 --name myjavaapp namrata21101998/javaweb:${BUILD_TAG}"
                    }

                }
            
        } 
    
    }
}
*/

pipeline {
    agent {
        label "linuxbuildnode"
        }

stages {
    stage ('SCM') {
        steps {
            git 'https://github.com/NamrataMajumdar21/jenkins-docker-maven-java-webapp.git'
            }
        }
    stage ('Build by Maven Package') {
        steps {
            sh 'mvn clean package'       
            }
        }
    stage ('Build Docker OWN image') {
        steps {
            sh "docker build -t  namrata21101998/javaweb:${BUILD_TAG} ."
            }
        }
    stage ('Push image to Docker HUB') {
        steps {
            withCredentials([string(credentialsId: 'DOCKER_HUB_PWD', variable: 'DOCKER_HUB_PASS_CODE')]) {
               sh "sudo docker login -u  namrata21101998 -p $DOCKER_HUB_PASS_CODE"
                sh "sudo docker push  namrata21101998/javaweb:${BUILD_TAG}" 
                }
            }
        }
    stage ('Deploy webApp in DEV env') {
        steps {
            sh 'sudo docker rm -f myjavaapp'
            sh "sudo docker run -d -p 8080:8080 --name myjavaapp namrata21101998/javaweb:${BUILD_TAG}"
            }
        }
    stage ('Deploy webApp in QA Test env') {
        steps {
            sshagent(['QA_ENV_SSH_CRED']) {
                sh "ssh -o StrictHostKeyChecking=no ec2-user@13.232.108.218 sudo docker rm -f myjavaapp"
                sh "ssh ec2-user@13.232.108.218 sudo docker run -d -p 8080:8080 --name myjavaapp namrata21101998/javaweb:${BUILD_TAG}"
                    }
                }
            }
    stage ('QAT Testing') {
        steps {
            retry(10) {
                sh 'curl --silent http://13.232.108.218:8080/java-web-app/ | grep india'
                    }
                }
            }
    stage('approved') {
            steps {
            script {
                Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                echo 'userInput: ' + userInput

                if(userInput == true) {
                    // do action
                } else {
                    // not do action
                    echo "Action was aborted."
                        }
                
                }
                }
            }
    stage('Deploy webAPP in Prod Env') {
            steps {
               sshagent(['QA_ENV_SSH_CRED']) {
                sh "ssh -o StrictHostKeyChecking=no ec2-user@3.108.52.98 sudo kubectl delete all --all"
                sh "ssh ec2-user@3.108.52.98 sudo kubectl create deployment myjavawebapp --image=namrata21101998/javaweb:${BUILD_TAG}"
                sh "ssh ec2-user@3.108.52.98 sudo kubectl apply -f https://raw.githubusercontent.com/NamrataMajumdar21/jenkins-docker-maven-java-webapp/refs/heads/master/webappsvc.yml"
                sh "ssh ec2-user@3.108.52.98 sudo kubectl scale deployment myjavawebapp --replicas=5"
                    }

                }
            
        } 
    
    }
}