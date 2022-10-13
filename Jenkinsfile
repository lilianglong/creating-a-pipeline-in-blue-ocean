pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      parallel {
        stage('Test') {
          environment {
            CI = 'true'
          }
          steps {
            sh './jenkins/scripts/test.sh'
          }
        }

        stage('OBS upload') {
          steps {
            huaweicloudOBSUpload(bucketName: 'obs-jenkins', endpoint: 'obs.cn-south-1.myhuaweicloud.com', localPath: '/', remotePath: '/jenkins/blueocean', maxRetries: '3', accessKeyId: 'TIFED7B7YLSOB3DRQIS8', secretAccessKey: '8UXZc5xc7AqXcnQ8NWNF5q1Cm6RWuPncevdqXCUm')
          }
        }

      }
    }

    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }

  }
}
