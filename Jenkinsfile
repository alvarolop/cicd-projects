node('maven') {

    def OPENSHIFT_PROJECT = "workshop-project-devops"
    def GIT_URL = "https://github.com/alvarolop/cicd-projects.git"
    def GIT_BRANCH = "master"

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
            git branch: GIT_BRANCH, url: GIT_URL
        }
    }

    stage('Create resources') {
        openshift.withCluster() {
            openshift.withProject(OPENSHIFT_PROJECT) {
                echo "Creating application..."
                openshift.apply ("-f", "/tmp/git-clone/hello-world-template.yml")
                openshift.apply( openshift.process( "hello-world", "-p", "APPLICATION_NAME=hello-world", "-p", "APPLICATION_NAMESPACE=hello-world" ))
            }
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