pipeline {
    agent {
        label "generic"
    } //agent
    stages {

        stage("Set up") {
            steps {
                sh """
                    python3 -m venv env
                    source ./env/bin/activate 
                    python -m pip install molecule
                    python -m pip install docker
                """
            } //steps
        } //stage

        stage("Create docker image for testing") {
            steps {
                sh """
                    source ./env/bin/activate 
                    molecule create
                """
            } //steps
        } //stage
        stage("Apply motd role to docker image") {
            steps {
                sh """
                    source ./env/bin/activate 
                    molecule converge
                """
            } //steps
        } //stage
        stage("Check idempotency") {
            steps {
                sh """
                    source ./env/bin/activate 
                    molecule idempotence
                """
            } //steps
        } //stage
        stage("Cleanup molecule") {
            steps {
                sh """
                    source ./env/bin/activate 
                    molecule cleanup
                """
            } //steps
        } //stage
        stage("Destroy molecule instance") {
            steps {
                sh """
                    source ./env/bin/activate 
                    molecule destroy
                """
            } //steps
        } //stage
    } //stages
    post {
        always {
            sh """
                rm -rf env
            """
        }
    }
} //pipeline
