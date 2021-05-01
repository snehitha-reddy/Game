  pipeline {
   agent any

   tools {
      // Install the Maven version configured as "M3" and add it to the path.
	  jdk 'JAVA'
      maven "MAVEN"
   }
   
   stages
   {
   stage('checkout') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/uday0007/project-game-of-life.git'
        }
        
        }
	}
}
