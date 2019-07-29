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
       stage('Docker image build') {
            steps {
                echo '..... Copying Artifacts & Building Docker image :: ......'
                sh 'cp java_web_code/target/wildfly-spring-boot-sample-1.0.0.war docker'
                sh 'cd docker && sudo docker build -t devops_pipeline_demo .'
                 }
        }
        stage('Running containers') {
            steps {
                echo '..... Copying Artifacts & Building Docker image :: ......'
               
               /* script {
               def CONTAINER= "devops_pipeline_demo"
           
               println "Value of container" + CONTAINER
 
               def RUNNING="${(sudo docker inspect --format="{{ .State.Running }}" devops_pipeline_demo 2> /dev/null)}"

               println "Value of running" + (RUNNING)

               //if ( (RUNNING) == false ){
                 if ( "$?" -eq 1) {
               echo "'CONTAINER' does not exist."
               } else {
               sh 'sudo docker rm -f (CONTAINER)'} */
               echo ""
	       echo "..... Deployment Phase Started :: Building Docker Container :: ......"
               sh 'cd docker && sudo docker rm -f devops_pipeline_demo'
               sh 'cd docker && sudo docker run -d -p 8180:8080 --name devops_pipeline_demo devops_pipeline_demo'
               echo "--------------------------------------------------------"
               echo "View App deployed here: http://server-ip:8180/sample.txt"
               echo "--------------------------------------------------------"
              // }
             }
        }
              
        
    }
}
