pipeline {
    agent any

    environment {
        PORT = '1601'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Khajushaik/Portfolio.git'
            }
        }

        stage('Verify Files') {
            steps {
                sh '''
                    echo "Checking portfolio files..."
                    ls -la
                    test -f index.html
                    test -f style.css
                    echo "Portfolio files verified successfully"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Starting portfolio on port ${PORT}..."

                    # Stop any existing server on port 1601
                    pkill -f "python3 -m http.server ${PORT}" || true

                    # Start website
                    nohup python3 -m http.server ${PORT} \
                        --bind 0.0.0.0 \
                        > portfolio.log 2>&1 &

                    sleep 3

                    echo "Portfolio started:"
                    curl -I http://localhost:${PORT}
                '''
            }
        }
    }

    post {
        success {
            echo "Portfolio deployed successfully on port 1601"
            echo "Open: http://<JENKINS-SERVER-IP>:1601"
        }

        failure {
            echo "Portfolio deployment failed"
        }
    }
}
