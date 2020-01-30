pipeline {
  agent {
    node {
      label 'jenkins-buildkit'
    }

  }
  stages {
    stage('Build Conda') {
      when {
        anyOf {
          expression {
            return params.FORCE_BUILD_CONDA
          }

          changeset '**/esgf_search/**'
        }

      }
      environment {
        CONDA_TOKEN = credentials('conda-token')
      }
      steps {
        container(name: 'buildkit', shell: '/bin/sh') {
          sh '''#! /bin/sh

make build-search'''
        }

      }
    }

    stage('Build Container') {
      when {
        anyOf {
          expression {
            return params.FORCE_BUILD_JUPYTER
          }

          changeset 'Dockerfile'
          changeset '**/esgf_search/**'
        }

      }
      steps {
        container(name: 'buildkit', shell: '/bin/sh') {
          sh '''#! /bin/sh

make build-jupyterhub'''
        }

      }
    }

  }
  parameters {
    booleanParam(name: 'FORCE_BUILD_JUPYTER', defaultValue: false, description: 'Force building container')
    booleanParam(name: 'FORCE_BUILD_CONDA', defaultValue: false, description: 'Force building conda package')
  }
}
