pipeline {
    agent {
        label "generic"
    } //agent
    stages {

        stage("Set up") {
            steps {
                sh """
                    sudo pip3 install molecule
                    sudo pip3 install docker
                """
            } //steps
        } //stage

        stage("Create docker image for testing") {
            steps {
                sh """
                    molecule create
                """
            } //steps
        } //stage
        stage("Apply motd role to docker image") {
            steps {
                sh """
                    molecule converge
                """
            } //steps
        } //stage
        stage("Check idempotency") {
            steps {
                sh """
                    molecule idempotence
                """
            } //steps
        } //stage
        stage("Cleanup molecule") {
            steps {
                sh """
                    molecule cleanup
                """
            } //steps
        } //stage
        stage("Destroy molecule instance") {
            steps {
                sh """
                    molecule destroy
                """
            } //steps
        } //stage
    } //stages
    post {
        always {
            sh """
                sudo pip3 uninstall docker -y
                sudo pip3 uninstall molecule -y
            """
        }
    }
} //pipeline
