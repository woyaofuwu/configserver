// Set your project Prefix using your GUID
def prefix = "andrew"

// Set variable globally to be available in all stages
// Set Maven command to always include Nexus Settings
def mvnCmd = "mvn -s ./cicd-settings-nexus3.xml"
// Set Development and Production Project Names
def devProject = "dev-${prefix}"
def prodProject = "prod-${prefix}"
// Set the tag for the development image: version + build number
def devTag = "0.0-0"
// Set the tag for the production image: version
def prodTag = "0.0"
def destApp = "tasks-green"
def activeApp = ""

pipeline {
    agent {
        label "maven"
    }
    options {
        skipDefaultCheckout()
        disableConcurrentBuilds()
    }
    stages {
// Checkout Source Code and calculate Version Numbers and Tags
stage('Checkout Source') {
steps {
// Replace the credentials with your credentials.
git  url: 'https://github.com/woyaofuwu/configserver.git'
// or when using the Pipeline from the repo itself:
 //checkout scm
 
}
}

// Using Maven build the war file
// Do not run tests in this step
/**stage('Build Jar File') {
steps {
//echo "Building version ${devTag}"
dir('depjar'){
     sh "${mvnCmd} clean package -DskipTests=true"
}
}
}**/




// Publish the built war file to Nexus
stage('Publish to Nexus') {
steps {
echo "Publish to Nexus"
//dir('depjar'){  //it's optional
    //  sh "${mvnCmd}  deploy -DskipTests=true -DaltDeploymentRepository=nexus::default::http://nexus3.nexus.svc.cluster.local:8081/repository/maven-releases"
//}
}
}

//compile a springboot app
stage('Compile springboot') {
steps {
echo "Compile springboot"
//dir('springboothello'){
      sh "${mvnCmd}  clean package -DskipTests=true "
//}
}
}

// Build the OpenShift Image in OpenShift and tag it.
stage('Build and Tag OpenShift Image') {
     steps {
          echo "Building OpenShift container image "

        // TBD: Build Image, tag Image
        //dir('springboothello'){
               script {
             openshift.withCluster() {
               openshift.withProject("dev-andrew") {
                 openshift.selector("bc", "hellotest").startBuild("--from-file=./target/configserver-0.0.1-SNAPSHOT.jar", "--wait=true")
       
                 //openshift.tag("hellotest:latest", "hellotest:v12")
               }
             }
           }
    //}
     }
}


}
}
