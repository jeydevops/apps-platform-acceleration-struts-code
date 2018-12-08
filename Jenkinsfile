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
          pushToCloudFoundry(
          target: "${env.pcfApiUrl}",
          credentialsId: 'pcfcreds',
          organization: "${env.pcfDevOrg}",
          cloudSpace: "${env.pcfDevSpace}",
          manifestChoice: [manifestFile: 'manifest.yml']
        )

}

}
