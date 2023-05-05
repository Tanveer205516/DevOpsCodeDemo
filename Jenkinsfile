pipeline
{
    tools
    {
       
        maven 'mymaven'
        ansible 'myansible'
    }
	agent any

    stages
    {
        stage('Checkout')
        {
            //agent{ label 'linux_slave'}
            steps
            {
                
                echo 'cloning'
                git 'https://github.com/sanjeev206/DevOpsCodeDemo.git'
              }
          }
        stage('Compile')
        {
            //agent any
            steps
            {
                //agent any
                echo 'complie the code..'
                sh 'mvn compile'
            }
        }
        stage('CodeReview')
        {
            //agent any
            steps
            {
                //agent{ label 'linux_slave'}
                echo 'codeReview'
                sh 'mvn pmd:pmd'
            }
        }
        stage('UnitTest')
        {
            //agent any
            steps
            {
                //agent any
                sh 'mvn test'
            }
        }
        stage('Package')
        {
            //agent any
            steps
            {
                //agent any
                sh 'mvn package'
            }
        }
        stage('build docker image')
        {
            steps
            {
                //sh 'chmod 777 /var/run/docker.sock'
                sh 'sudo cp /var/lib/jenkins/workspace/project1pipeline/target/addressbook.war .'
                sh 'docker build -t myproject1image .'

            }
        }
        stage('Push image to dockerhub')
        {
            steps
            {
                
                withCredentials([string(credentialsId: 'dockerhub_pwd', variable: 'dockerhub_pwd')]) 
                {
                    sh 'docker login -u sanju206 -p ${dockerhub_pwd}'
                }
                sh 'docker tag myproject1image sanju206/myproject1image'
                sh 'docker push  sanju206/myproject1image'
                
            }
        }
        stage('deployment via Ansible')
        {
            steps
            {
                ansiblePlaybook become: true, credentialsId: 'ansible_client', disableHostKeyChecking: true, installation: 'myansible', inventory: 'demo.inv', playbook: 'playbook.yml'

            }
        }
    }
}
