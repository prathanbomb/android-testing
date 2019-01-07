pipeline {
  agent any
  stages {
    stage('test') {
      steps {
        sh './gradlew test'
      }
    }
    stage('pack .apk') {
      steps {
        sh './gradlew assembledDebug'
      }
    }
    stage('archive') {
      steps {
        archiveArtifacts(artifacts: '**/*.apk', allowEmptyArchive: true, fingerprint: true)
        junit(allowEmptyResults: true, testResults: '**/*.xml')
      }
    }
  }
}