pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Mohamed-KBIBECH/PainCare_Frontend_Angular.git', branch: 'main'
                echo 'Checkout completed.'
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
                            -Dsonar.projectKey=voiture ^
                            -Dsonar.host.url=http://localhost:9001 ^
                            -Dsonar.login=sqp_9dc557fb5de0a58f14b82109bd15b5b5bb10a102 ^
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

