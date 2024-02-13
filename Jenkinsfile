podTemplate(
    cloud: 'kubernetes', 
    containers: [
        containerTemplate(
            name: 'maven', 
            image: 'adoptopenjdk/maven-openjdk8', 
            command: 'sleep', 
            args: '30d', 
            ttyEnabled: true
        )
    ],
    volumes: [
        persistentVolumeClaim(
            mountPath: '/root/.m2/repository', 
            claimName: 'jenkins-pv-claim', 
            readOnly: false
        )
    ]
) {
    node(POD_LABEL) {
        stage('Get a Maven project') {
            git 'https://github.com/yballaj/umlDevOps.git'
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
