# About Jenkinsfile 

1. Automate pipeline setup with Jenkinsfile
2. Jenkinsfile defines the stages in the CI/CD pipeline.
3. Jenkinsfile is a text file with pipeline DSL Syntax 
4. DSL is Similar to groovy 
5. Two Syntax Types
    - Scripted
    - Declarative 
6. Pipeline Concepts 
    - Pipeline
    - Node/Agent
    - Stage
    - Step 

# Jenkinsfile Syntax 
pipeline {
    agent any
    stages {
        stage('Build'){
            steps {
                //
            }
        }
        stage('Test') {
            steps {
                //
            }
        }
        stage('Deploy'){
            steps {
                //
            }
        }
    }
}