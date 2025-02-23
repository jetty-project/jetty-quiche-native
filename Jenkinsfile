#!groovy

pipeline {
  agent any
  options {
    durabilityHint('PERFORMANCE_OPTIMIZED')
    buildDiscarder(logRotator(numToKeepStr: '7', artifactNumToKeepStr: '2'))
    timeout(time: 60, unit: 'MINUTES')
  }
  stages {
    stage( "Parallel Stage" ) {
      parallel {
        stage( "Build / Test - JDK11" ) {
          agent { node { label 'linux-light' } }
          options { timeout( time: 120, unit: 'MINUTES' ) }
          steps {
            mavenBuild( "jdk11", "clean install javadoc:javadoc" )
            // Collect the JaCoCo execution results.
            recordCoverage name: "Coverage ${jdk}", id: "coverage-${jdk}", tools: [[parser: 'JACOCO']], sourceCodeRetention: 'MODIFIED',
                        sourceDirectories: [[path: 'src/main/java']]
            warnings consoleParsers: [[parserName: 'Maven'], [parserName: 'Java']]
            script {
            echo "env.BRANCH_NAME:" + env.BRANCH_NAME
              if ( env.BRANCH_NAME == 'main' )
              {
                mavenBuild( "jdk11", "deploy" )
              }
            }
          }
        }
      }
    }
  }
}

/**
 * To other developers, if you are using this method above, please use the following syntax.
 *
 * mavenBuild("<jdk>", "<profiles> <goals> <plugins> <properties>"
 *
 * @param jdk the jdk tool name (in jenkins) to use for this build
 * @param cmdline the command line in "<profiles> <goals> <properties>"`format.
 * @return the Jenkinsfile step representing a maven build
 */
def mavenBuild(jdk, cmdline) {
  def mvnName = 'maven3'
  def localRepo = "${env.JENKINS_HOME}/${env.EXECUTOR_NUMBER}" // ".repository" //
  def settingsName = 'oss-settings.xml'
  def mavenOpts = '-Xms2g -Xmx2g -Djava.awt.headless=true'

  withMaven(
          maven: mvnName,
          jdk: "$jdk",
          publisherStrategy: 'EXPLICIT',
          globalMavenSettingsConfig: settingsName,
          options: [junitPublisher(disabled: false)],
          mavenOpts: mavenOpts,
          mavenLocalRepo: localRepo) {
    // Some common Maven command line + provided command line
    sh "mvn -V -B $cmdline"
  }
}

// vim: et:ts=2:sw=2:ft=groovy
