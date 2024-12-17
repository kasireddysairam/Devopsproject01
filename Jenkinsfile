
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


        
      
}
