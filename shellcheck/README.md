This docker image wraps [shellcheck](https://github.com/koalaman/shellcheck) as a single executable.

## Build the image

    cd shellcheck/docker
    docker build -t shellcheck .

Also, this images is already published to Docker Hub, so you can pull it directly,

    docker pull soulmachine/shellcheck


## How to use it

For example, you have a shell script named `sample.sh` under the current directory, you can lint it by running:

    docker run -v $(pwd):/tmp/work soulmachine/shellcheck /tmp/work/sample.sh

