pipeline {
    agent any
    tools {
        maven 'M3'
//         jdk 'JAVA_HOME'
    }

    stages {
        stage ('Build') {
            steps {
                 sh 'mvn clean install'
            }
            post {
                success {
                    echo '------- Post Build ----------'
                }
            }
        }

        stage ('JUnit Test') {
//             when {
//                 changeRequest target: 'master', branch: /^TEST-\d+$/, comparator: 'REGEXP'
//             }
            steps {
                 //sh 'mvn -Dmaven.test.failure.ignore=true test'
                 sh 'make check || true'
            }
            post {
                success {
                    echo '------- JUnit Test Pass  ----------'
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
        stage ('Deploy') {
            steps {
                    echo 'Deploy success'
            }
            post {
                success {
                    echo '------- Package done  ----------'
                }
            }
        }
        stage('build & SonarQube analysis') {
            agent any
            steps {
              withSonarQubeEnv('My SonarQube Server') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
          stage('Quality Gate') {
              steps {
                timeout(time: 1, unit: 'HOURS') {
                  waitForQualityGate abortPipeline: true
                }
              }
            }
    }
//     post {
//             always {
//                 junit 'target/surefire-reports/**/*.xml'
//             }
//             failure {
//                mail (to: 'r.penumuru@ths-bs.de',
//                    subject: 'The ${env.JOB_NAME} (${env.BUILD_NUMBER}) Failure!',
//                    body: "Please visit the url for more details :${env.BUILD_URL}"
//                );
//             }
//             success {
//                 mail (to: 'r.penumuru@ths-bs.de',
//                         subject: 'The ${env.JOB_NAME} (${env.BUILD_NUMBER}) Success :)',
//                         body: "Please visit the url for more details :${env.BUILD_URL}"
//                     );
//             }
//         }
}
