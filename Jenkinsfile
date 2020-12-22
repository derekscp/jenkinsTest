pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                //git branch: 'master', url: "https://github.com/derekscp/xraytest.git"
                git branch: 'Original', url: "https://github.com/derekscp/xraytest.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: 'ARTIFACTORY_SERVER',
                    url: 'http://localhost:8081/artifactory',
                    username: 'test',
                    password: 'password123'
                )

                rtMavenDeployer (
                    id: 'MAVEN_DEPLOYER',
                    serverId: 'ARTIFACTORY_SERVER',
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: 'ARTIFACTORY_SERVER',
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshot"
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'Maven', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install -U',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
        
        /*stage ('Xray scan') {
            steps {
                xrayScan (
                    serverId: "ARTIFACTORY_SERVER",
                    failBuild: true
                )
            }
        }*/
    }
}
