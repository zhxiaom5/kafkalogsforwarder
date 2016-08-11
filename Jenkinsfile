#!groovy

node {
    stage 'Test stuff'

    withEnv(["service=foo", "bar=baz"]) {
        sh "env"
    }
    withEnv(["service=${env.JOB_NAME.split('/')[0]}", "branch=${env.BRANCH_NAME}"]) {
        sh "env"
    }
    withEnv(['service=$(echo foo)']) {
        sh "env"
    }
    withEnv(['service=${sh "echo foo"}']) {
        sh "env"
    }
    withEnv(["service=${sh 'echo bar'}"]) {
        sh "env"
    }

    stage 'Build static binary' 
    sh '''
        service=$(echo $JOB_NAME | cut -d/ -f 1)
        branch=$BRANCH_NAME
        revision=$(git log --format="%H" -n 1)
        docker_image=registry2.applifier.info:5005/$service:$revision

        docker run --rm -v $PWD:/go/src/github.com/UnityTech/kafkalogsforwarder -w /go/src/github.com/UnityTech/kafkalogsforwarder golang:latest /bin/bash -c "go get -u github.com/kardianos/govendor; govendor sync; go build -a -ldflags \'-s\' -tags netgo -installsuffix netgo -v -o kafkalogsforwarder && ! ldd kafkalogsforwarder"
    '''
    sh "env"

    if (env.BRANCH_NAME != "master") {
        stage 'Print stuff'
        echo "foo"
        sh "env"
    }

    if (env.BRANCH_NAME == "master") {
        stage 'Build the image'
        echo "bar"
        sh "env"
    }
}
