pipeline{
    agent any
    environment{
        SONAR_HOME=tool "Sonar"
    }
    stages{
        
        stage("Pull Code"){
            steps{
                git url : "https://github.com/PrathmeshAdhav2006/DevSecOps-wanderlust.git",branch:"main"
            }
        }
        
        stage("SonarQube Quality"){
            steps{
                withSonarQubeEnv("Sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=Wanderlust -Dsonar.projectKey=wanderlust"
                }
            }
        }
        
        stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./' , odcInstallation:'OWASP'
                dependencyCheckPublisher pattern: "**/dependency-check-report.html"
            }
        }
        
        stage("Sonar Quality Gate Scan"){
            steps{
                timeout(time: 2, unit: "MINUTES"){
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        
        stage("Deploy"){
            steps{
                 sh 'docker compose down'
                 sh 'docker compose up -d --build'
            }
        }
    }
}
