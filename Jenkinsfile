pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = 'AKIAW3MEFE3GJDVIJANP'
        AWS_SECRET_ACCESS_KEY = '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'
    }

    agent any
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    dir("terraform") {
                        echo 'Cloning repository...'
                        git branch: 'main', url: 'https://github.com/iamironman4279/terraforms.git'
                        echo 'Repository cloned successfully.'
                    }
                }
            }
        }

        stage('Verify Environment') {
            steps {
                sh 'echo $PATH'
                sh 'terraform --version'
            }
        }

        stage('Plan') {
            steps {
                sh 'pwd; cd terraform/ ; terraform init'
                sh 'pwd; cd terraform/ ; terraform plan -out tfplan'
                sh 'pwd; cd terraform/ ; terraform show -no-color tfplan > tfplan.txt'
            }
        }

        stage('Approval') {
            when {
                not {
                    equals expected: true, actual: params.autoApprove
                }
            }

            steps {
                script {
                    def plan = readFile 'terraform/tfplan.txt'
                    input message: "Do you want to apply the plan?",
                          parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                }
            }
        }

        stage('Apply') {
            steps {
                sh 'pwd; cd terraform/ ; terraform apply -input=false tfplan'
            }
        }
    }
}
