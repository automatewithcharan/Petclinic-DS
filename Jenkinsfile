pipeline {
    agent any 
    
    tools{
        jdk 'jdk17'
        maven 'maven3.8.7'
    }
    
    // environment {
    //     SCANNER_HOME=tool 'sonar-scanner'
    // }
    
    stages{
        
        stage("Git Checkout"){
            steps{
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/jaiswaladi246/Petclinic.git'
            }
        }
        
        stage("Compile"){
          steps {
                script {
                    def jdkPath = tool name: 'jdk17', type: 'hudson.model.JDK'
                    env.JAVA_HOME = jdkPath
                    env.PATH = "${jdkPath}/bin:" + env.PATH
                }

                sh '''
                    echo "JAVA_HOME is: $JAVA_HOME"
                    PATH=$JAVA_HOME/bin:$PATH
                    export PATH
                    which java
                    which javac
                    java -version
                    javac -version
                    mvn clean compile
                '''
            }
        }
        
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
        
        // stage("Sonarqube Analysis "){
        //     steps{
        //         withSonarQubeEnv('sonar-server') {
        //             sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Petclinic \
        //             -Dsonar.java.binaries=. \
        //             -Dsonar.projectKey=Petclinic '''
    
        //         }
        //     }
        // }
        
        // stage("OWASP Dependency Check"){
        //     steps{
        //         dependencyCheck additionalArguments: '--scan ./ --format HTML ', odcInstallation: 'DP'
        //         dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }
        
         stage("Build"){
            steps{
                sh " mvn clean install"
            }
        }
        
        // stage("Docker Build & Push"){
        //     steps{
        //         script{
        //            withDockerRegistry(credentialsId: 'f0037abb-e18b-4523-b9eb-b9f8d8317a6f', toolName: 'docker') {
                        
        //                 sh "docker build -t image1 ."
        //                 sh "docker tag image1 pillinayan/pet-clinic123:latest "
        //                 sh "docker push pillinayan/pet-clinic123:latest "
        //             }
        //         }
        //     }
        // }
        
        // stage("TRIVY"){
        //     steps{
        //         sh 'trivy clean --java-db'
        //         sh 'trivy image --timeout 5m pillinayan/pet-clinic123:latest'
        //     }
        // }
        
        stage("Deploy To Tomcat"){
            steps {
    sh '''
      echo "Current directory: $(pwd)"
      cp target/petclinic.war /opt/tomcat/webapps/
    '''
  }
        }
    }
}
