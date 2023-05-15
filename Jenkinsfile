pipeline
{
	agent any

    stages
    {
        stage('Cloning code from Git')
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
        stage('push code to test server and image to docker Hub')
        //stage('build docker image')
        {
            //steps
            //{
                parallel
                {
                    stage('docker hub')
                    {
                        steps{
                            //sh 'chmod 777 /var/run/docker.sock'
                            sh 'sudo cp /var/lib/jenkins/workspace/project1pipeline/target/addressbook.war .'
                            sh 'docker build -t myproject1image .'
                            withCredentials([string(credentialsId: 'dockerhub_pwd', variable: 'dockerhub_pwd')])
                                {
                                    sh 'docker login -u ahmadtanveer80 -p ${dockerhub_pwd}'
                                }
                            sh 'docker tag myproject1image ahmadtanveer80/myproject1image'
                            sh 'docker push  ahmadtanveer80/myproject1image'
                        }
                    }

                    stage('Test deployment')
                    {
                        steps
                        {
                            ansiblePlaybook become: true, credentialsId: 'edureka', disableHostKeyChecking: true, installation: 'myansible', inventory: 'demo.inv', playbook: 'playbook.yml'
                        }
                    }
                }
            //}
        }
    }
}
