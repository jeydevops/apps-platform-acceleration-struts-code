env.dockerimagename="devopsbasservice/buildonframework:buildon-cf-plugin-1.0"
node {
   stage ('Build') {
   //If some other Repository is to be given apart from current repo, provide git URL as below demo...
    checkout scm
    sh 'mvn clean package -DskipTests=True'
  }   
   
   stage ('Deploy to PCF') {  
    pushToCloudFoundry(
  target: 'api.system.dev.digifabricpcf.com',
  credentialsId: 'pcfcreds',
  organization: 'cde-product-integration',
  cloudSpace: 'cde-product-integration',  
  manifestChoice: [
    value: 'jenkinsConfig',
    appName: 'struts-example',
    memory: 512,
    instances: 1,
    appPath: 'target/struts.war'
  ]
)
  }  
}
