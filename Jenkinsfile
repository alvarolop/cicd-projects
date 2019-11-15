node('maven') {

    def OPENSHIFT_PROJECT = "devops"
    def GIT_URL = "git@github.com:alvarolop/cicd-projects.git"
    def GIT_BRANCH = "master"
    def GIT_CREDENTIALS = "devops-gitlab"

    stage("Check project ${OPENSHIFT_PROJECT} exists") {
        openshift.withCluster() {
            // Check if project exists and create it if not
            if(!openshift.selector("project", OPENSHIFT_PROJECT).exists()){
                openshift.newProject(OPENSHIFT_PROJECT)
                echo "Project ${OPENSHIFT_PROJECT} did not exist. Created"
            }

            // Check current project
            openshift.withProject(OPENSHIFT_PROJECT) {
                echo "Using project ${openshift.project()}"
            }
        }
    }



    stage("Clone application sources") {
        dir ('/tmp/git-clone') {
            echo "git clone -b ${GIT_BRANCH} ${GIT_URL}"
            git branch: GIT_BRANCH, credentialsId: GIT_CREDENTIALS, url: GIT_URL
        }
    }

    stage('Build') {
        openshift.withCluster() {
            openshift.withProject(OPENSHIFT_PROJECT) {
                echo "Building..."
            }
        }
    }

    stage('Test') {
        openshift.withCluster() {
            openshift.withProject(OPENSHIFT_PROJECT) {
                echo "Testing..."
            }
        }
    }

    input message: "Approve Deploy?", ok: "Yes"

    stage('Deploy') {
        openshift.withCluster() {
            openshift.withProject(OPENSHIFT_PROJECT) {
                echo "Deploying..."
            }
        }
    }
}