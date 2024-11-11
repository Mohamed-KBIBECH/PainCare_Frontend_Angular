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
                nodejs('NodeJs') {
                    bat 'npm install'
                    echo 'NPM install completed.'
                }
            }
        }
        stage('Build Angular') {
            steps {
                script {
                    nodejs('NodeJs') {
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
                        bat """
                            ${scannerHome}\\bin\\sonar-scanner.bat ^
                            -sonar.projectKey=SonarTest ^
                            -sonar.host.url=http://localhost:9001 ^
                            -sonar.login=sqp_2ddb46cd7e4170c82727c2b91993afaddf8064a1 ^
                            -sonar.sources=src ^
                            -sonar.exclusions="**/node_modules/**"
                          
                        """
                        echo 'SonarQube analysis completed.'
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
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
