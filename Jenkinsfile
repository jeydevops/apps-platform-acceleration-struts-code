env.dockerimagename="devopsbasservice/buildonframework:spabuildon-pcf"
node {

   stage ('Build') {
     checkout scm
     sh 'mvn clean package -DskipTests=True'
  }

stage ('Dev_Deployment') {

          sh "mvn -B help:evaluate -Dexpression=project.build.finalName | grep -e '^[^[]' > finalNameFile"
          projectName=readFile('finalNameFile').trim()

          sh "mvn -B help:evaluate -Dexpression=project.packaging | grep -e '^[^[]' > packagingFile"
          packaging=readFile('packagingFile').trim()
          sh "echo '  path: target/${projectName}.${packaging}' >>manifest.yml"
          pcfApiUrl = sh (script: 'echo $pcfApiUrl',returnStdout: true).trim()
          pcfDevOrg = sh (script: 'echo $pcfDevOrg',returnStdout: true).trim()
          pcfDevSpace = sh (script: 'echo $pcfDevSpace',returnStdout: true).trim()


   
        pushToCloudFoundry(
           target: ${pcfApiUrl},
          credentialsId: 'devpcfcreds',
           organization: ${pcfDevOrg},
           cloudSpace: ${pcfDevSpace},
          manifestChoice: [manifestFile: 'manifest.yml']
        )

}

}
