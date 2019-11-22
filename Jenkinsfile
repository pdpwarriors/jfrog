node {
    def server = Artifactory.server('pdpwarriors.jfrog.io')
    def buildInfo = Artifactory.newBuildInfo()
    def rtMaven = Artifactory.newMavenBuild()
    
    
    stage ('Code Checkout') {
        git url: 'https://github.com/Fullmavencode/jfrog.git'
    }
 
    stage ('Code Build') {
        rtMaven.tool = 'Maven-3.6.1' // Tool name from Jenkins configuration
        rtMaven.run pom: 'pom.xml', goals: 'clean compile'
    }
    
    stage ('UnitTest') {
        rtMaven.tool = 'Maven-3.6.1' // Tool name from Jenkins configuration
        rtMaven.run pom: 'pom.xml', goals: 'test'
    }
    
   // stage('SonarQube Analysis') {
       // withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.6.1') {
         //   rtMaven.run pom: 'pom.xml'
     // sh 'mvn sonar:sonar'
            
   // }
         
    //  }
    
    stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage..:
         
        rtMaven.tool = 'Maven-3.6.1' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        rtMaven.deployer.deployArtifacts = false // Disable artifacts deployment during Maven run
     }
            
    stage ('Install') {
        rtMaven.run pom: 'pom.xml', goals: 'install', buildInfo: buildInfo
     }
 
    stage ('Deploy') {
        rtMaven.deployer.deployArtifacts buildInfo
    }
        
    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
    
    stage ('Deploy to Dev') {
        
    }
       
}
