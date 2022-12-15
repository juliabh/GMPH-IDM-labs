# GMPH-IDM-LABS

This repository contains all of the information needed to construct the learner environment for IDM labs on Coursera. The key thing here is that this repo is the source of truth - the can be no updating notebooks directly in Coursera or in a different location without pushing to this repo, or it defeats the point!

The repo can be broken up into a few key parts:

- This file is the **readme** for the repo, which you can refer back to for info. The readme is concerned with the technical aspects of managing the notebooks on Coursera as opposed to about the contents of the notebooks themselves - please raise an issue here in this repo or speak to Julia Halder (j.halder09@imperial.ac.uk) directly
- The files in the **labs** folder contain all of the files to be included inside labs in Coursera. In this course, all of these files are available to learners at some point
- The files contained in the **learner-environment** folder are used to construct the image for the labs to live on in Coursera

## Coursera labs basics

#TODO

In this section we review some basic aspects of Coursera labs. If you already have experience with uploading to labs and linking them to items, feel free to skip!

## IDM Labs

IDM features many notebooks, all captured within the **labs** directory inside this repository. They are broken up into 3 chunks, "IDM1", "IDM2" and "IDM3", to draw some equivalence between the degree course and the three MOOCs (each chunk corresponds to a MOOC). We didn't break them up further because we wanted to allow learners to have a backend environment they can navigate within inside each chunk of the course.

Each IDMx folder corresponds to a lab that lives in the [Lab Manager](https://www.coursera.org/teach/idm-gmph/I5fSExyvEe23EQpSrOmLeQ/content/labs) on Coursera. To upload a "chunk" to the lab manager I recommend making a zip file of the contents of the IDMx folder in question, uploading the zip inside the new lab on Coursera, and creating a new terminal in the Jupyter interface. Then the commands:

    unzip IDMx.zip
    rm IDMx.zip
    exit

will populate the lab. The "x" will need to be replaced with whatever number you use.

The structure inside each IDMx folder on Coursera needs to be maintained as the same as the structure here, as the notebooks load files from specific locations using hardcoded file paths.

## Learner Environment

The learner environment configuration is captured in the Dockerfile within the **learner-environment** folder in this repo. The Dockerfile is used to generate an image in the Coursera lab manager.

Each line of the file provides step-by-step instructions to build an environment that will run the learner notebooks and the Jupyter interface, like a recipe. When you upload the dockerfile to Coursera, it follows these instructions to produce an "image". Each lab starts as a copy of this image with the ability to host and maintain files and changes to those files, and then we load the extra files that are appropriate for that lab into it. When the learner launches their lab, they're launching their own personal version of this lab.

This technology is based on Docker, which you will need if you want to run the environment locally for testing, for example when updating the environment. I use Docker desktop on Windows - see the [Docker docs](https://docs.docker.com/get-docker/) for information for how to install and configure. One important thing to note is that, at time of writing, Windows users need to install Windows Subsystem for Linux 2, WSL2, in order to run the docker commands below. Instructions for how to do that are available in the [Windows WSL docs](https://learn.microsoft.com/en-us/windows/wsl/install).

With docker set up, to build the image run the following command in the command line:

    docker build -t gmph-idm-labs/learner-env -f learner-environment/Dockerfile .

To run the image use the following

    docker run -p 8888:8888 gmph-idm-labs/learner-env

then navigate to `localhost:8888` in your browser.

In doing so, you'll find you have an empty copy of the learner environment on your browser, in much the same way as when you click "Test Image" in the Coursera lab manager. It's a good place to run a file checking that the right libraries are available without having to upload to Coursera. Feel free to experiment with attaching the different labs folders as volumes or other Docker-related fancy things, but I never felt it was worth the effort!

Don't forget to stop the docker container running once you're done. I usually do this in Docker desktop.

The required_packages.csv file in the learner-environment folder is the list of the packages specifically requested by Julia for IDM. This is captured from the learner environment, so these libraries are available and have the properties listed. This can be used as a reference when updating the Dockerfile.