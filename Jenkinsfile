podTemplate(containers: [
    containerTemplate(
        name: 'maven',
        image: 'adoptopenjdk/maven-openjdk8',
        command: 'sleep',
        args: '30d',
        volumeMounts: [
            volumeMount(
                name: 'maven-repo',
                mountPath: '/root/.m2/repository'
            )
        ]
    )
],
volumes: [
    persistentVolumeClaim(
        claimName: 'jenkins-pv-claim',
        mountPath: '/root/.m2/repository',
        name: 'maven-repo'
    )
]) {
    node(POD_LABEL) {
        stage('Get a Maven project') {
            git 'https://github.com/dlambrig/simple-java-maven-app.git'
            container('maven') {
                stage('Build a Maven project') {
                    sh '''
                    echo "maven build"
                    mvn -B -DskipTests clean package
                    '''
                }
            }
        }
    }
}
