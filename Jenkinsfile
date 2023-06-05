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
    stage('test') {
      steps {
        echo 'Hello'
      }
    }

  }
}
