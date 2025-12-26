pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    environment {
        K8S_NAMESPACE = "automotive"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/codercorp/namespace.git'
            }
        }

        stage('Create / Update Namespace') {
            steps {
                sh '''
                    echo "Applying Kubernetes namespace..."
                    kubectl apply -f namespace.yaml
                '''
            }
        }

        stage('Verify Namespace') {
            steps {
                sh '''
                    echo "Verifying namespace..."
                    kubectl get namespace ${K8S_NAMESPACE}
                '''
            }
        }
    }

    post {

        success {
            emailext(
                subject: "✅ SUCCESS: Kubernetes Namespace Ready",
                mimeType: 'text/html',
                body: """
                    <h2 style="color:green;">Namespace Created / Verified 🎉</h2>
                    <p><b>Namespace:</b> automotive</p>
                    <p>
                        <b>Jenkins Job:</b>
                        <a href="${BUILD_URL}">${BUILD_URL}</a>
                    </p>
                """,
                to: "erdigvijaypatil01@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "❌ FAILURE: Namespace creation failed",
                mimeType: 'text/html',
                body: """
                    <h2 style="color:red;">Namespace Creation Failed ❌</h2>
                    <p><b>Namespace:</b> automotive</p>
                    <p>
                        <b>Check Jenkins logs:</b>
                        <a href="${BUILD_URL}">${BUILD_URL}</a>
                    </p>
                """,
                to: "erdigvijaypatil01@gmail.com"
            )
        }
    }
}
