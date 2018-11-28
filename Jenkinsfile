env.dockerimagename="devopsbasservice/buildonframework:buildon-cloudfoundry"
node {
   stage ('Build') {
   //If some other Repository is to be given apart from current repo, provide git URL as below demo...
    checkout scm
    sh 'mvn clean package -DskipTests=True'
  }   
 
   stage ('Dev_Deployment') {
          //Projectname
          sh "mvn -B help:evaluate -Dexpression=project.build.finalName | grep -e '^[^[]' > finalNameFile"
          projectName=readFile('finalNameFile').trim()
   
          //Packagename
          sh "mvn -B help:evaluate -Dexpression=project.packaging | grep -e '^[^[]' > packagingFile"
          packaging=readFile('packagingFile').trim()
        
         //sh "echo '  path: target/${projectName}.${packaging}' >>DevManifest.yml"
            def yaml = readYaml file: "Devmanifest.yml"
            print yaml.applications.path
            yaml['applications'][0]['path'] = "target/${projectName}.${packaging}"
            sh 'rm Devmanifest.yml'
            writeYaml file:"Devmanifest.yml", data:yaml

        pushToCloudFoundry(
          target: 'api.system.cumuluslabs.io',
          credentialsId: 'devpcfcreds',
          organization: 'forward-engineering',
          cloudSpace: 'dev',
          manifestChoice: [manifestFile: 'DevManifest.yml']
        )
}
  
}
