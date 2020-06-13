FROM jupyterhub/singleuser

# Setting root user
USER root


ENV r_studio_version=1.3.959

# Adding conda to $PATH 
ENV PATH=/opt/conda/bin:$PATH

# Fixing permissions and environment and password requests
RUN rm /etc/environment &&\
    echo  "PATH=\"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/opt/conda/bin\"" >> /etc/environment &&\
    echo 'jovyan  ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Installing latest R version and RStudio
RUN apt-get update && apt-get -y upgrade && apt-get install --no-install-recommends -y vim gnupg2 gdebi-core &&\
    echo 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/' >> /etc/apt/sources.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 &&\
    apt-get update &&  apt-get install  --no-install-recommends -y r-base build-essential &&\
    wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-$r_studio_version-amd64.deb  &&\
    sudo gdebi --non-interactive rstudio-server-$r_studio_version-amd64.deb &&\
    rm rstudio-server-$r_studio_version-amd64.deb &&\
    echo "session-default-working-dir=/home/jovyan/work" >> /etc/rstudio/rsession.conf &&\
    echo "session-default-new-project-dir=/home/jovyan/work" >> /etc/rstudio/rsession.conf &&\
    pip install --no-cache-dir jupyter-rsession-proxy ipyparallel &&\
    apt-get remove -y gnupg2 gdebi-core && apt-get -y autoremove &&  apt-get clean && rm -rf /var/lib/apt/lists/*

# Fixing permissions
RUN chmod 777 -R /usr/local/lib/R/site-library &&\
    echo "cd ~/work" >> /home/jovyan/.bashrc


# Reverting back to jovyan user
USER  $NB_UID


# # Installing most used R packages
# COPY R_packages.txt /home/jovyan/