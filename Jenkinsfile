pipeline {
    agent any

    tools {
        nodejs 'NodeJS'  // Name must match the NodeJS installation configured in Jenkins
    }

    environment {
        CI = 'true'
        REPO_URL = 'https://github.com/JOHOTechy/ecom-frontend.git'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url: "${REPO_URL}"
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                bat 'npm run lint'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying to production...'
                // Example: copy build to a web server
                // bat 'xcopy /E /Y dist\\* \\\\server\\www\\zetop\\'
                //
                // Or deploy to Netlify / Vercel / S3:
                // bat 'npx netlify-cli deploy --prod --dir=dist'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above.'
        }
        always {
            cleanWs()  // Clean workspace after build
        }
    }
}
