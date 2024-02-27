podTemplate(containers: [
    containerTemplate(
        name: 'gradle', image: 'gradle', command: 'sleep', args: '30d'
    )],
    // Set pod retention policy to retain the pod only on failure
    podRetention: onFailure()
) {

    node(POD_LABEL) {
        stage('Run pipeline against a gradle project') {
            container('gradle') {
                stage('Build a gradle project') {
                    git 'https://github.com/yballaj/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
                    sh '''
                    cd Chapter08/sample1
                    chmod +x gradlew
                    '''
                }
            
                stage("Tests") {
                    try {
                        sh '''
                        pwd
                        cd Chapter08/sample1
                        ./gradlew test
                        ./gradlew jacocoTestReport
                        ./gradlew checkstyleMain checkstyleTest
                        '''
                    } catch (Exception E) {
                        echo 'Failure detected'
                    }

                    publishHTML(target: [
                        reportDir: 'Chapter08/sample1/build/reports/tests/test',
                        reportFiles: 'index.html',
                        reportName: "JaCoCo Report"
                    ])

                    publishHTML(target: [
                        reportDir: 'Chapter08/sample1/build/reports/checkstyle',
                        reportFiles: 'main.html', 
                        reportName: "JaCoCo Checkstyle Report"
                    ])
                }
            }
        }
    }
}
