
pipeline {
    agent any
parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git Branch Name')
        string(name: 'GIT_URL' ,defaultValue: 'https://github.com/kasireddysairam/Devopsproject01.git',description: 'Git Repo url')
        string(name: 'MAVEN_PROFILE', defaultValue: 'default', description: 'Maven Profile')
    }



    
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
       git branch: params.BRANCH_NAME, url: params.GIT_URL
        
    }
}

}
stage("3. Maven Unit Test") {  
            // Test the individual units of code 
            steps{
                script{
                    sh 'mvn test -Dmaven.test.skip=${params.SKIP_TESTS}'
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
