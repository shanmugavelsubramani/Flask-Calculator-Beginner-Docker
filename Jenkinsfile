pipeline {
  agent { docker { image 'python:3.8.2' } }
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'echo "building the repo"'
//             echo 'Creating virtualenv ...'
//             sh 'virtualenv --no-site-packages .env'
//
//             sh """
//                    . .env/bin/activate
//                    pip install -r requirements.txt
//                    """
          }
        }
      }
    }

    stage('Test') {
      steps {
      sh 'pip3 install -r requirements.txt'
        sh 'python3 test_app.py'
        input(id: "Deploy Gate", message: "Deploy ${params.project_name}?", ok: 'Deploy')
      }
    }

    stage('Deploy')
    {
      steps {
        echo "deploying the application"
      }
    }

  }

  post {
        always {
            echo 'The pipeline completed'
            junit allowEmptyResults: true, testResults:'**/test_reports/*.xml'
        }
        success {
            
            sh "sudo nohup python3 app.py > log.txt 2>&1 &"
            echo "Flask Application Up and running!!"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}
