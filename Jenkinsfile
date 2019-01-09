pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh './gradlew clean assemble assembleAndroidTest'
      }
    }
    stage('test') {
      steps {
        sh './gradlew test'
      }
    }
    stage('archive') {
      steps {
        archiveArtifacts(artifacts: '**/*.apk', fingerprint: true)
        junit(testResults: '**/*.xml', allowEmptyResults: true)
      }
    }
    stage('test lab') {
      steps {
        sh 'gcloud firebase test android run firebase-test-matrix.yml:Pixel2-device --app app/build/outputs/apk/mock/debug/app-mock-debug.apk --test app/build/outputs/apk/androidTest/mock/debug/app-mock-debug-androidTest.apk --project digioci'
      }
    }
    stage('publish apk') {
      steps {
        androidApkUpload apkFilesPattern: '**/app-prod-release.apk', googleCredentialsId: 'Google Play Android Developer', rolloutPercentage: '100', trackName: 'internal'
      }
    }
  }
  post {
    failure {
      sh "curl https://notify-api.line.me/api/notify -H 'Authorization: Bearer kFrfPqbAvIg4KeXBWKMRRppHyJCsb3tPGTGqeg6XNKN' -F 'message=${env.JOB_NAME} #${env.BUILD_NUMBER} \nresult is failed. \n${env.BUILD_URL}' -F 'stickerId=173' -F 'stickerPackageId=2'"
    }
    success {
      sh "curl https://notify-api.line.me/api/notify -H 'Authorization: Bearer kFrfPqbAvIg4KeXBWKMRRppHyJCsb3tPGTGqeg6XNKN' -F 'message=${env.JOB_NAME} #${env.BUILD_NUMBER} \nresult is succeed. \n${env.BUILD_URL}' -F 'stickerId=525' -F 'stickerPackageId=2'"
    }
  }
}
