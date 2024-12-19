
pipeline {
    agent any
parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git Branch Name')
        string(name: 'GIT_URL' ,defaultValue: 'https://github.com/kasireddysairam/Devopsproject01.git',description: 'Git Repo url')
         string(name: 'ENVIRONMENT', defaultValue: 'dev', description: 'Target Environment')
    }
environment {
        DOCKER_USERNAME = "sairamk1998"     // check the 'ID' in your Jenkins credentials
    }

    
    stages {
        stage('1.cleanup') {
            steps {
                // Clean workspace directory for the current build
            
                deleteDir ()   
            }
        }
    

    stage('2.Git Checkout'){
         when {
        expression {
            // Your condition here, e.g., checking a parameter value:
            return (params.BRANCH_NAME == 'main') && (params.ENVIRONMENT == 'dev')
        }
    }
    steps {
        script {
       git branch: params.BRANCH_NAME, url: params.GIT_URL
        
    }
}

}
stage("3. Maven Unit Test") {  
            // Test the individual units of code 
            steps{
                script{
                    sh 'mvn test '
                }
            }
        }

        stage('4. Maven Build') {
            // Build the application into an executable file (.jar)
            steps{
               script{
                  sh 'mvn clean install'   
               } 
            }
        }

        stage("5. Maven Integration Test") {
            //  Test the interaction between different units of code
            steps{
               script {
                  sh 'mvn verify'          
               }
            }
        }

    stage('6.Docker Image Build'){
        steps{
            script {
             def JOB = env.JOB_NAME.toLowerCase() 
             def   BuildNo = env.BUILD_NUMBER
             echo "$BuildNo"
             sh  "docker  build -t  ${JOB}:${BuildNo} ."
                
            }
        }
    }

 stage('7. Docker Image Tag') {
            // Rename the Docker Image before pushing to Dockerhub
            steps{
                     // go to directory where Docker Image is created
                  script {
                    def JOB = env.JOB_NAME.toLowerCase() // Convert Jenkins Job name to lower-case
                    sh "docker tag ${JOB}:${BUILD_NUMBER} ${DOCKER_USERNAME}/${JOB}:v${BUILD_NUMBER}"
                    sh "docker tag ${JOB}:${BUILD_NUMBER} ${DOCKER_USERNAME}/${JOB}:latest"
                  }
                }
            } 

stage('8. Trivy Image Scan') {
            // Scan Docker images for vulnerabilities 
            steps{
                script { 
                  def JOB = env.JOB_NAME.toLowerCase() // Convert Jenkins Job name to lower-case
                  sh "trivy image --scanners vuln  ${DOCKER_USERNAME}/${JOB}:v${BUILD_NUMBER} > scan.txt"
                }
            }
        }


        stage('9. Docker Image Push') {
            // Login to Dockerhub & Push the image to Dockerhub
            steps{
                script { 
                withCredentials([usernamePassword(credentialsId: 'my_dockerhub_creds', passwordVariable: 'docker-pass', usernameVariable: 'docker_user')]) {
                    sh "docker login -u '${docker_user}' -p '${docker_pass}'"
                    def JOB = env.JOB_NAME.toLowerCase() // Convert Jenkins Job name to lower-case
                    sh "docker push ${DOCKER_USERNAME}/${JOB}:v${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_USERNAME}/${JOB}:latest"
                  }
                }
            }
        }

         stage('10. Docker Image Cleanup') {
            // Remove the unwanted (dangling) images created in Jenkins Server to free-up space
            steps{
                script { 
                  sh "docker image prune -af"
                }
            }
        }


    }
        
      
}
