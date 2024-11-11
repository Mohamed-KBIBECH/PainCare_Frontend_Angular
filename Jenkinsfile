pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Mohamed-KBIBECH/PainCare_Frontend_Angular.git', branch: 'main'
                echo 'Checkout completed.'
            }
        }
        stage('Install Node.js') {
            steps {
                echo 'Starting Node.js installation...'
                nodejs('NodeJs') {
                    bat 'npm install'
                }
                echo 'Node.js installation completed.'
            }
        }
        stage('Build Angular') {
            steps {
                script {
                    nodejs('NodeJs') {
                        echo 'Starting Angular build...'
                        bat 'npm run build --prod'
                        echo 'Angular build completed.'
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner';
                    withSonarQubeEnv('SonarQube') {
                        echo 'Starting SonarQube analysis...'
                        bat """
                            ${scannerHome}\\bin\\sonar-scanner.bat ^
                            -Dsonar.projectKey=SonarTest ^
                            -Dsonar.host.url=http://localhost:9001 ^
                            -Dsonar.login=sqp_2ddb46cd7e4170c82727c2b91993afaddf8064a1 ^
                            -Dsonar.sources=src ^
                            -Dsonar.exclusions="**/node_modules/**"
                        """
                        echo 'SonarQube analysis completed.'
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                echo 'Checking quality gate...'
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
                echo 'Quality gate check completed.'
            }
        }
    }
    post {
        always {
            echo "Build process completed."
        }
        failure {
            echo "Build failed."
        }
        success {
            echo "Build succeeded."
        }
    }
}

