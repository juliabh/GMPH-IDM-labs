FROM jupyter/r-notebook:2022-11-21

RUN fix-permissions /home/jovyan/.jupyter

USER $NB_USER

# Give human write access to these directories
RUN fix-permissions $CONDA_DIR
RUN fix-permissions /home/$NB_USER

# Install required packages (anything not here was already installed in the base image)
RUN Rscript -e "devtools::install_version('deSolve', '1.34', repos='https://cran.ma.imperial.ac.uk')"

USER root

# Set environment variable to launch notebook instead of lab
ENV DOCKER_STACKS_JUPYTER_CMD=notebook

# Set entry point
EXPOSE 8888
WORKDIR /home/$NB_USER/work
ENTRYPOINT ["tini", "--"]
# Avoid requiring permissions by setting notebook token as empty
CMD ["start-notebook.sh", "--NotebookApp.token=''"]

USER $NB_USER