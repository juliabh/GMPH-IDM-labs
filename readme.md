# GMPH-IDM-LABS


## Learner Environment

The learner environment configuration is captured in the Dockerfile in this repo, which can be used to generate an image in the Coursera lab manager.

To run this environment locally for testing, Docker is required - see the [Docker docs](https://docs.docker.com/get-docker/) for information for how to install and configure this.

With docker set up, to build the image run the following command in the command line:

    docker build -t gmph-idm-labs/learner-env -f learner-environment/Dockerfile .

To run the image use the following

    docker run -p 8888:8888 gmph-idm-labs/learner-env

then navigate to `localhost:8888` in your browser.