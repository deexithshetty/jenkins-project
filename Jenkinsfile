pipeline{

agent any

tools {
maven 'maven3.9.5'
}
  
triggers {
// add the trigger options
  pollSCM '* * * * *'
}


parameters {
  choice choices: ['dev', 'qa', 'stage', 'preprod ', 'prod'], description: 'env_name', name: 'env_name'
}

options {
timestamps()
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '2', numToKeepStr: '5')
}


stages{

stage('Checkout stage'){
steps{
// Pulls code from the 'dev' branch using stored GitHub credentials
git branch: params.env_name, credentialsId: 'githubcreds', url: 'https://github.com/deexithshetty/jenkins-project.git'
}
}

stage('Compile Stage'){
steps{
 // Compiles the Java source code into .class files
sh 'mvn compile'
}
}

stage('Test Stage'){
steps{
// Runs unit tests to validate the code
sh 'mvn test'
}
}

stage('Trivy report'){
steps{
// Generates trivy report
sh 'trivy fs --format table -o trivy_report.html .'
}
}

stage('Sonarqube report'){
steps{
// Generates Sonarqube report
sh 'mvn sonar:sonar'
}
}

stage('Create war file'){
steps{
// Packages the compiled code and resources into a WAR file
sh 'mvn package'
}
}

stage('Store WAR File Locally'){
steps{
// Installs the WAR file into your local Maven repository (~/.m2/repository)
sh 'mvn install'
}
}

/*
stage('Deploy Stage'){
steps{
// Deploys the WAR file to a remote repository
sh 'mvn deploy'
}
*/



} // stages closing
}  // pipeline closing
