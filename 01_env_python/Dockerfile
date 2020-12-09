FROM jupyterhub/singleuser

# Setting root user
USER root

# Adding conda to $PATH 
ENV PATH=/opt/conda/bin:$PATH

# Fixing permissions and environment and password requests
RUN rm /etc/environment &&\
    echo  "PATH=\"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/opt/conda/bin\"" >> /etc/environment &&\
    echo 'jovyan  ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Installing vim
RUN apt-get update && apt-get -y upgrade && apt-get install -y --no-install-recommends\
    vim \
    pandoc \
    texlive-xetex \
    texlive-fonts-recommended &&\
    apt-get -y autoremove &&\
    apt-get clean && rm -rf /var/lib/apt/lists/*


# Installing jupyter_contrib_nbextensions and its components
## Updating conda before all installations
RUN conda update -y --all &&\
    conda install -c conda-forge jupyter_contrib_nbextensions \
    jupyter_nbextensions_configurator \
    ipywidgets\
    nbconvert &&\
    conda clean -y -a

# Adding frequently used and recommended python packages
## Copying file package list from machine
COPY python_packages.txt /home/jovyan/
RUN pip install --no-cache-dir -r /home/jovyan/python_packages.txt &&\
    rm /home/jovyan/python_packages.txt

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
    jupyter nbextension enable code_prettify/autopep8 &&\
    echo "cd ~/work" >> /home/jovyan/.bashrc