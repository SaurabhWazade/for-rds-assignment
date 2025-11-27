pipeline {
    agent any

    tools {
        maven "apache-maven-3.9.11"
    }

    stages {

        stage ('git clone') {
            steps {
                dir('/mnt/') {
                    sh """
                    rm -rf project || true
                    git clone https://github.com/SaurabhWazade/project.git

                    cd /mnt/servers/apache-tomcat-10.1.49/webapps/
                    rm -rf LoginWebApp* || true
                    """
                }
            }
        }

        stage ('maven') {
            steps {
                dir('/mnt/project') {
                    sh "mvn clean package"
                }
            }
        }

        stage ('extract war file') {
            steps {
                sh """
                cd /mnt/
                rm -rf test || true
                mkdir test
                cd test/
                mkdir LoginWebApp
                cd LoginWebApp/
                cp -r /mnt/project/target/LoginWebApp.war .
                unzip LoginWebApp.war
                rm -rf LoginWebApp.war
                """
            }
        }

        stage ('edit userregistration file') {
            steps {
                dir ('/mnt/test/LoginWebApp') {
                    sh """
                    sed -i 's|"jdbc:mysql://localhost:3306/test", "root", "root"|"jdbc:mysql://database-1.cl2ge2kg8jsb.ap-south-1.rds.amazonaws.com:3306/test", "admin", "ratan1234"|g' userRegistration.jsp
                    cd ..
                    zip -r LoginWebApp.war LoginWebApp
                    rm -rf LoginWebApp
                    cp LoginWebApp.war /mnt/servers/apache-tomcat-10.1.49/webapps/
                    """
                }
            }
        }

    }

}
