pipeline {
    agent any

      tools { 
        maven 'Maven-3' 
        jdk 'Java-8' 
       }

      stages {

       stage ('Initialize the build variables') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
        stage('Building the app') {
            steps {
                echo '..... Build Phase Started :: Compiling Source Code :: ......'
                sh 'cd java_web_code && mvn install'
                }
        }
        stage('Testcases execution') {
            steps {
                echo '..... Test Phase Started :: Testing via Automated Scripts :: ......'
                sh 'cd integration-testing && mvn clean verify -P integration-test'
                 }
        }
       stage('Ansible deployment to host') {
            steps {
                echo '..... Initiating ansible deployment to host system :: ......'
                //sh 'ansible-playbook -i hosts main.yml --ask-become-pass'
                  sh 'cd docker && export ANSIBLE_HOST_KEY_CHECKING=False && ansible-playbook -i hosts main.yml --extra-vars "ansible_user=jenkins ansible_password=Appuzz@2019" '
                  }
        }
                    
        
    }
}
