pipeline {
  agent {
    kubernetes {
      // Define the pod template with container template
      containerTemplate {
            name 'gradle'
            image 'gradle'
            command 'sleep'
            args '30d'
        }
    }
  }
  
  stages {
    stage('Checkout code and prepare environment') {
      steps {
        git url: 'https://github.com/bingbingkuai/chp5.git', branch: 'master'
        sh ''' 
          cd Chapter08/sample1
          chmod +x gradlew
        ''' 
      }
    }

    stage('Run pipeline against a gradle project - main branch') {
      when {
        branch 'main'
      }
      steps {
         echo 'On main branch'

         sh ''' 
         pwd
         cd Chapter08/sample1
         ./gradlew test
         ./gradlew jacocoTestCoverageVerification
         '''
      }
    }

    stage('Run pipeline against a gradle project - other branches') {
      when {
         not { branch 'main' }
      }
      steps {
        echo 'Unit test NOT on main branch'
        script {
          try {
                sh ''' 
                  pwd
                  cd Chapter08/sample1; 
                  ./gradlew checkstyleMain
                ''' 
                echo 'Test PASS!!!'
          } catch (Exception E) {
                echo 'Oh no. Test FAIL!!!'
          }
        }
          //HTML publisher plugin
          publishHTML (target: [
              reportDir: 'Chapter08/sample1/build/reports/checkstyle',
              reportFiles: 'checkstyle.xml',
              reportName: "JaCoCo Checkstyle"
          ])  
      }
    }
  }
  post { 
    always {
      echo 'pipeline completed' 
    }
  }
}
