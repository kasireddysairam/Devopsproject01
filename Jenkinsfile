
pipeline {
    agent any

    stages {
        stage('1.cleanup') {
            steps {
                // Clean workspace directory for the current build
            
                deleteDir ()   
            }
        }
    

    stage('2.Git Checkout'){
    steps {
        script {
       git branch: 'main', url: 'https://github.com/kasireddysairam/Devopsproject01.git'
        
    }
}

}
stage("3. Maven Unit Test") {  
            // Test the individual units of code 
            steps{
                script{
                  sh 'mvn test'        
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
