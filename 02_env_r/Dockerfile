FROM jupyterhub/singleuser

# Setting root user
USER root

# Adding conda to $PATH 
ENV PATH=/opt/conda/bin:$PATH

# Fixing permissions and environment and password requests
RUN rm /etc/environment &&\
    echo  "PATH=\"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/opt/conda/bin\"" >> /etc/environment &&\
    echo 'jovyan  ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Installing latest R versions
RUN apt-get update && apt-get -y upgrade && apt-get install --no-install-recommends -y vim \
    pandoc \
    texlive-xetex \
    texlive-fonts-recommended \
    texlive-generic-recommended \
    gnupg2 &&\
    echo 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/' >> /etc/apt/sources.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 &&\
    apt-get update &&  apt-get install --no-install-recommends -y r-base build-essential libzmq3-dev libcurl4-openssl-dev libssl-dev jupyter-core jupyter-client &&\
    apt-get remove -y gnupg2 && apt-get -y autoremove &&  apt-get clean && rm -rf /var/lib/apt/lists/*

# Installing IRkernel
RUN R -e "install.packages(c('repr', 'IRdisplay', 'IRkernel'), type = 'source')" && \
    R -e "IRkernel::installspec(user = FALSE)"

# Installing jupyter_contrib_nbextensions and its components
## Updating conda before all installations
RUN conda update -y --all &&\
    conda install -c conda-forge jupyter_contrib_nbextensions \
    jupyter_nbextensions_configurator \
    ipywidgets \
    nbconvert &&\
    conda clean -y -a &&\
    pip install --no-cache-dir ipyparallel


# Fixing permissions
RUN chmod 777 -R /usr/local/lib/R/site-library &&\
    chmod 777 -R /home/jovyan/.local

# Reverting back to jovyan user
USER  $NB_UID


COPY jupyter_config/common.json /home/jovyan/.jupyter/nbconfig/ 

## Enabling most used NB extensions
RUN sudo chmod 777 -R /home/jovyan/.jupyter &&\
    jupyter nbextension enable spellchecker/main && \
    jupyter nbextension enable execute_time/ExecuteTime && \
    jupyter nbextension enable notify/notify && \
    jupyter nbextension enable skip-traceback/main && \
    jupyter nbextension enable scratchpad/main && \
    jupyter nbextension enable snippets_menu/main && \
    jupyter nbextension enable init_cell/main && \
    jupyter nbextension enable collapsible_headings/main && \
    jupyter nbextension enable toc2/main && \
    jupyter nbextension enable toggle_all_line_numbers/main && \
    jupyter nbextension enable hinterland/hinterland && \
    jupyter nbextension enable codefolding/main && \
    jupyter nbextension enable livemdpreview/livemdpreview && \
    jupyter nbextension enable varInspector/main && \
    jupyter nbextension enable tree-filter/index && \
    jupyter nbextension enable autosavetime/main &&\
    echo "cd ~/work" >> /home/jovyan/.bashrc



# # Installing most used R packages
# COPY R_packages.txt /home/jovyan/