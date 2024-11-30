pipeline { 
  agent any 

    tools { 
      maven 'maven' 
      jdk 'jdk17'
      nodejs 'node16'
    }

        
 stages{
    stage('Clean Workspace'){
        steps{
            cleanWs()
            }
        }
    stage('Checkout from Git'){
        steps{
            git branch: 'main', url: 'https://github.com/eyabenltaief/CI-CD.git'
            }
        }
    stage("Compile"){
        steps{
            sh "mvn clean compile"
        }
    }
        
    stage("Test Cases"){
        steps{
            sh "mvn test"
        }
    }
    stage("OWASP Dependency Check"){
        steps{
            dependencyCheck additionalArguments: '', odcInstallation: 'DP-check'
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        }
    }
    
    
    stage('Build') {
      steps { 
        sh 'mvn clean install' 
      }   
    }
    
    
    
    stage("Docker Build"){
        steps{
            script{
               withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {
                    sh "docker build -t image1 . "
                    sh "docker tag image1 eyabenltaief/pet-clinic:latest "
                }
            }
        }
    }
    
    stage('TRIVY') {
      steps{
            sh "trivy image eyabenltaief/pet-clinic:latest --format json -o trivy-docker-image-scan.json" 
        }
    }
  
 
    stage('Upload Report to DefectDojo') {
        steps {
            s 'pip install requests'
            sh 'python /home/eya/API-DefectDojo/DefectDojo.py'
    }
    }


     stage("Docker Push"){
        steps{
            script{
               withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {
                    sh "docker push eyabenltaief/pet-clinic:latest "
                }
            }
        }
    }

    }    
}
