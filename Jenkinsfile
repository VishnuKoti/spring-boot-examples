#!groovy

def releasedVersion

node('master') {
  def dockerTool = tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
  withEnv(["DOCKER=${dockerTool}/bin"]) {
  


 
    stage("Build"){
     		git url: 'https://github.com/VishnuKoti/spring-boot-examples.git'
                dir('spring-boot-tutorial-basics'){
                def mvnHome = tool 'M3'
    		 sh "${mvnHome}/bin/mvn clean package"
                     dockerCmd 'build --tag springguy/springguy:SNAPSHOT .'
            }
    }

   
    
      stage('Deploy') {
            stage('DeployAgain') {
                 dir('spring-boot-tutorial-basics'){
                    dockerCmd 'run -d -p 9990:9990 --name "springshot" --network="host" springguy/springguy:SNAPSHOT'
                }
            }
    }
    
    
    
  }
}

def dockerCmd(args) {
    sh "${DOCKER}/docker ${args}"
}

def getReleasedVersion() {
    return (readFile('pom.xml') =~ '<version>(.+)-SNAPSHOT</version>')[0][1]
}