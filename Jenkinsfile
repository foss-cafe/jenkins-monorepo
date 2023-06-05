pipeline {
    options {
        ansiColor('xterm')
        timestamps () 
      }
  environment { 
        DOCKER_BUILDKIT = '1'
    }

  agent {
    kubernetes {
      //cloud 'kubernetes'
      defaultContainer 'docker'
      yaml '''
        kind: Pod
        spec:
          volumes:
          - name: docker-socket
            emptyDir: {}
          containers:
          - name: docker
            image: docker:19.03.1
            command:
            - sleep
            args:
            - 99d
            volumeMounts:
            - name: docker-socket
              mountPath: /var/run
          - name: docker-daemon
            image: docker:19.03.1-dind
            securityContext:
              privileged: true
            volumeMounts:
            - name: docker-socket
              mountPath: /var/run
          - name: trivy
            image: aquasec/trivy
            command:
            - sleep
            args:
            - 99d
            securityContext:
              privileged: true
            volumeMounts:
            - name: docker-socket
              mountPath: /var/run
'''
    }
  }
  stages {
        stage('Build Frontend') {
            when {
                anyOf {
                changeset "**/frontend/**"
                }
            }
            
            steps {
                container("docker") {
                sh "docker build  -t frontend -f frontend/Dockerfile ."
                sh "docker images"
            }
            }
        }

        stage('Build Backend') {
            when {
                anyOf {
                changeset "**/backend/**"
                }
            }
            
            steps {
                container("docker") {
                sh "docker build  -t backend -f backend/Dockerfile ."
                sh "docker images" 
            }
            }
        }
    }
}
