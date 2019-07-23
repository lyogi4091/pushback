properties([parameters([booleanParam(defaultValue: true, description: 'select to format the code', name: 'Autopep8')])])
node {
    stage('Building Docker image'){
        git 'git@github.com:lyogi4091/Exercise-3.git'
        sh 'sudo docker build -t ubuntu_image_remotely .'
        sh 'sudo docker run -it -d --name testcontainer_remote_exercise3 -v /home/ciuser/Exercise-3:/opt ubuntu_image_remotely'
                }
        try{
            sh 'sudo docker exec testcontainer_remote_exercise3 cat /opt/python.py'
            sh 'sudo docker exec testcontainer_remote_exercise3 pycodestyle --ignore=E501 /opt/python.py'
            println "Code is already in good format"
            }catch(z){
                if (Boolean.parseBoolean("${env.Autopep8}")){
                    sh 'sudo docker exec testcontainer_remote_exercise3 autopep8 -i /opt/python.py'
                    println "Code is now formatted"
                }
                }
    stage('Pushing the formatted code'){
        dir('/home/ciuser/Exercise-3'){
            try{
			    sh 'sudo git add python.py'
			    sh 'sudo git commit -m "Commit after autopep8"'
			    sh 'sudo git push origin master'
			    }catch (d){
			        echo "No changes found to push."
			        }
        }
    }
}
