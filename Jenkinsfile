podTemplate(yaml: '''
  apiVersion: v1
  kind: Pod
  spec:
    containers:
    - name: gradle
      image: gradle:6.3-jdk14
      command:
      - sleep
      args:
      - 99d
      volumeMounts:
      - name: shared-storage
        mountPath: /mnt
    - name: kaniko
      image: gcr.io/kaniko-project/executor:debug
      command:
      - sleep
      args:
      - 9999999
      volumeMounts:
      - name: shared-storage
        mountPath: /mnt
      - name: kaniko-secret
        mountPath: /kaniko/.docker
    - name: calculator
      image: dengel223/hello-kaniko:1.0
      command:
      - sleep
      args:
      - 99d
      volumeMounts:
      - name: shared-storage
        mountPath: /mnt
    restartPolicy: Never
    volumes:
    - name: shared-storage
      persistentVolumeClaim:
        claimName: jenkins-pv-claim
    - name: kaniko-secret
      secret:
        secretName: dockercred
        items:
        - key: .dockerconfigjson
          path: config.json
''') {
  node(POD_LABEL) {
    stage('Prep') {
      container('gradle') {
        stage('Prep Stage') {
          echo env.GIT_BRANCH
          sh '''          
          '''
        }
      }
    }
    stage('Packager Build a gradle project') {
      git  url: 'https://github.com/dengellive/week7.git' , branch: "main"
      container('gradle') {
        stage('Build a gradle project') {
          sh '''
          chmod +x gradlew
          ./gradlew build
          mv ./build/libs/calculator-0.0.1-SNAPSHOT.jar /mnt
          '''
        }
      }
    }
    stage('Calculate It') {
      container('calculator') {
        stage('Calculator Stage') {
          sh '''          
          '''
        }
      }
    }
  }
}