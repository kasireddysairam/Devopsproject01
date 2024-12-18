
pipeline {
    agent any
parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git Branch Name')
        string(name: 'GIT_URL' ,defaultValue: 'https://github.com/kasireddysairam/Devopsproject01.git',description: 'Git Repo url')
         string(name: 'ENVIRONMENT', defaultValue: 'dev', description: 'Target Environment')
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






        

    }
        
      
}
