node {
    stage('SCM Checkout') {
        git 'https://github.com/ashikali22/my-app.git'
    }

    stage('maven-buildstage') {
        def mvnHome = tool name: 'maven3', type: 'maven'
        sh "${mvnHome}/bin/mvn clean package"
        sh 'mv target/myweb*.war target/newapp.war'
    }

    stage('Build Docker Image') {
      sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 065943327701.dkr.ecr.ap-south-1.amazonaws.com'
      sh 'docker build -t ashikali22/myweb.0.0.2 .'
      sh 'docker tag ashikali22/myweb.0.0.2:latest 065943327701.dkr.ecr.ap-south-1.amazonaws.com/ashikali22/myweb.0.0.2:latest'
    }
    stage('Docker Image Push to ECR') {
              registryCredential = """\
                    
        """
        sh 'docker push 065943327701.dkr.ecr.ap-south-1.amazonaws.com/ashikali22/myweb.0.0.2:latest'
    }

    stage('Remove Previous Container') {
        try {
            sh 'docker rm -f tomcattest'
        } catch (error) {
            // Do nothing if there is an exception
        }
    }

    stage('Docker deployment') {
        sh 'docker push 065943327701.dkr.ecr.ap-south-1.amazonaws.com/ashikali22/myweb.0.0.2:latest'
    }
}
