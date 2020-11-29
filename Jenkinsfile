pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for development') {
            // about: when: If a branch condition’s value (i.e. pattern) matches the name of the branch that Jenkins is running the build from, then the stage that contains this "when" and branch construct will be executed.for more look at https://www.jenkins.io/doc/tutorials/build-a-multibranch-pipeline-project //
            when {
                branch 'development' 
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            // about: when: If a branch condition’s value (i.e. pattern) matches the name of the branch that Jenkins is running the build from, then the stage that contains this "when" and branch construct will be executed. for more look at https://www.jenkins.io/doc/tutorials/build-a-multibranch-pipeline-project //
            when {
                branch 'production'  
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}

// save this jenkinsfile in the root directory of your master branch and then run "git stage ." then git commit -m "Add 'Deliver…​' and 'Deploy…​' stages"//
