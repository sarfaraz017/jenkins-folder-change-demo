pipeline {
  agent any

  stages {
    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }

    stage('Detect latest file in src/') {
      when {
        changeset "src/**"
      }
      steps {
        script {
          // ensure repo is unshallowed for diff
          sh 'if git rev-parse --is-shallow-repository >/dev/null 2>&1; then git fetch --unshallow || true; fi'

          def changed = sh(
            script: "git diff --name-only HEAD~1 HEAD | grep '^src/' || true",
            returnStdout: true
          ).trim()

          if (!changed) {
            echo "⚠️ No changes in src/, skipping."
            currentBuild.result = 'SUCCESS'
            return
          }

          def files = changed.split('\\n')
          env.LATEST_FILE = files[-1].trim()
          echo "✅ Latest file changed in src/: ${env.LATEST_FILE}"
        }
      }
    }

    stage('Process latest file') {
      when {
        expression { return env.LATEST_FILE }
      }
      steps {
        echo "📂 Processing file: ${env.LATEST_FILE}"
        sh "cat ${env.LATEST_FILE} || true"
      }
    }
  }
}

