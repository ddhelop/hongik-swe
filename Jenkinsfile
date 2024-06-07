pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Git 리포지토리에서 코드를 체크아웃
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                script {
                    // 소스 파일을 컴파일
                    def sourceDir = "${env.WORKSPACE}/src/main/java/org/example"
                    def buildDir = "${env.WORKSPACE}/build/classes/java/main"
                    sh "mkdir -p ${buildDir}"
                    sh "javac -d ${buildDir} ${sourceDir}/*.java"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // 테스트 파일을 컴파일
                    def testDir = "${env.WORKSPACE}/build/classes/java/test"
                    def testSourceDir = "${env.WORKSPACE}/src/test/java/org/example"
                    def testOutputDir = "${env.WORKSPACE}/test-output"
                    sh "mkdir -p ${testDir} ${testOutputDir}"
                    sh "javac -cp ${buildDir} -d ${testDir} ${testSourceDir}/*.java"

                    // JUnit 5를 사용하여 테스트 실행 및 결과 저장
                    sh "java -cp ${testDir}:${buildDir}:~/.m2/repository/org/junit/platform/junit-platform-console-standalone/1.8.2/junit-platform-console-standalone-1.8.2.jar org.junit.platform.console.ConsoleLauncher --scan-class-path=${testDir} > ${testOutputDir}/test-results.txt"
                }
            }
        }

        stage('Archive Results') {
            steps {
                // 테스트 결과를 아카이브
                archiveArtifacts artifacts: 'test-output/test-results.txt', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // 항상 빌드 디렉터리 정리
            cleanWs()
        }
    }
}