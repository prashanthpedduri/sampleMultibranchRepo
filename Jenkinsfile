pipeline {
   agent any
   tools {
      maven 'Maven'
   }
   stages {
       stage("build") {
           steps {
              //snDevOpsStep()
                echo "Building" 
             //   sh 'mvn clean install -DskipTests'
              
               rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "my-local-artifactory",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
               )
              
               rtMavenRun (
                  deployerId: 'MAVEN_DEPLOYER',
                  // Tool name from Jenkins configuration.
                  tool: 'Maven',
                  pom: 'pom.xml',
                  goals: 'clean install',
                  opts: '-DskipTests',
                  buildName: "${env.JOB_NAME}",
                  buildNumber: "${env.BUILD_NUMBER} - ${env.STAGE_NAME}",
               )
              
              rtSetProps (
               serverId: 'my-local-artifactory',
               props: 'p1=v1;p2=v2',     
               spec: '''{
                 "files": [{
                     "pattern": "libs-snapshot-local/sndevops/*/*.jar",
                     "props": "buildNumber=${env.BUILD_NUMBER} - ${env.STAGE_NAME}"
                 }]}'''
              )
              
              rtPublishBuildInfo (
                 serverId: 'my-local-artifactory',
              )
           }
       }
       
     stage("test") {
           stages {
            stage('UAT unit test1') {
                steps {
                        //snDevOpsStep ()
                        echo "Testing"
                        sh 'mvn -Dtest=com.sndevops.eng.AppTest test'
                }                       
            }
            stage('UAT unit test 2') {
                 steps {
                         //snDevOpsStep ()
                        echo "Testing"
                        sh 'mvn -Dtest=com.sndevops.eng.App1Test test'                     
                }
            }     
        }
          post {
              always {
              junit '**/target/surefire-reports/*.xml' 
              }
        }
      
      }
           
      /*stage("test-1") {
                steps {
                        //snDevOpsStep ()
                        echo "Testing"
                       // sh 'mvn -Dtest=com.sndevops.eng.AppTest test'
                }                    
        }*/

       /*stage("deploy") {
         stages{
             stage('deploy UAT') {
                when{
                   branch 'dev'
                }
               steps{
                 //snDevOpsStep ()
                 echo "deploy in UAT"
               }
             }
            stage('deploy PROD') {
               when {
                  branch 'master'
               }
                steps{
                  //snDevOpsStep ()
                   echo "deploy in prod"
                  //snDevOpsChange()              
                }
            }
        }
      }*/
     
    }
     
  }
