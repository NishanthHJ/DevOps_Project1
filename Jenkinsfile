pipeline{
    agent any

    environment {
        GEMINI_API_KEY = credentials('GEMINI_API_KEY')
        PORT = '3000'
    }

    stages {
        stage( 'Checkout' ){
            steps {
                git branch: 'main',
                url: 'https://github.com/NishanthHJ/chatgpt_clone.git'
            }
        }

        stage( 'Build Docker Image' ) {
            steps {
                sh 'docker build -t  chatgpt_clone .'
            }
        }

        stage( 'Deploy App' ) {
            steps {
                script {
                    sh """
                        docker stop chatgpt-clone-container || true
                        docker rm chatgpt-clone-container || true
                        docker run -d \
                            --name chatgpt-clone-container \
                            -p 3000:3000 \
                            -e GEMINI_API_KEY='${GEMINI_API_KEY}' \
                            -e PORT='${PORT}' \
                            chatgpt_clone 
                    """
                }
            }
        }
    }
}