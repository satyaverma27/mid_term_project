pipeline{

    // Declare variables that will be used by the later stages
    environment {
        img=""
     
    }

    agent any

    stages{

        stage('Github pull')
        {
             steps {
                // credentials are required because its a private repository
                git url: 'https://github.com/satyaverma27/mid_term_project',
                branch: 'main'
             }
        }

        stage('Maven build +Junit')
        {
            steps{
                sh 'mvn clean install'

            }
        }

         stage('Build Docker Image') {
			steps {
				script{
				    img= docker.build ("satyaverma27/miniproject:latest")
				}
			}
		}

// 	stage('Login to Docker Hub') {
//       steps{
// 	    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
//      	echo 'Login Completed'
//     }
// }

		stage('push to dockerhub') {
			steps {
			    script{
			        docker.withRegistry("","jenkins"){
			        img.push()
			        }
			    }

			}
 		}
	    stage('Delete Docker image from local repository')
 		{
 		    steps
 		    {
 		        sh 'docker rmi satyaverma27/miniproject'
 		    }
 		}
	stage('Ansible Deploy') {
            steps {
               // sh 'export ANSIBLE_HOST_KEY_CHECKING=False'
                 withEnv(['LC_ALL=en_US.UTF-8', 'LANG=en_US.UTF-8']){
                //sh 'LANG=en_US.UTF-8'

                 //installation: 'Ansible', inventory: 'hosts', playbook: 'ping2.yml'
                 //disableHostKeyChecking:true,
                 ansiblePlaybook becomeUser: 'null',
                 colorized:true,
                 installation: 'Ansible',
                 inventory: 'inventory',
                 playbook: 'deploy.yml',
                 sudoUser: 'null'
                 }
            }
        }

    }
}
