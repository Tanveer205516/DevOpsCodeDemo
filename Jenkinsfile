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
                git 'https://github.com/Sonal0409/DevOpsClassCodes.git'
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
                sh 'sudo chmod 777 /var/run/docker.sock'
                sh 'docker built -t myproject1image'

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
            {
                ansiblePlaybook become: true, credentialsId: 'ansible_client', disableHostKeyChecking: true, installation: 'myansible', inventory: 'demo.inv', playbook: 'playbook.yml'

            }
        }
    }
}
