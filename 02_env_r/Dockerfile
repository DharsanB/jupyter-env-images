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
RUN apt-get update && apt-get -y upgrade && apt-get install -y vim gnupg2

# Installing latest R versions
RUN gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && \
    gpg -a --export E298A3A825C0D65DFD57CBB651716619E084DAB9 | sudo apt-key add - && \
    apt-get update &&  apt-get install -y r-base build-essential libzmq3-dev libcurl4-openssl-dev libssl-dev jupyter-core jupyter-client &&\
    apt-get -y autoremove &&  apt-get clean

# Installing R kernel for Jupyter
RUN R -e "install.packages(c('repr', 'IRdisplay', 'IRkernel'), type = 'source')" && \
    R -e "IRkernel::installspec(user = FALSE)"


# Fixing permissions
RUN chmod 777 -R /home/jovyan/.jupyter &&\
    chmod 777 -R /usr/local/lib/R/site-library &&\
    chmod 777 -R /home/jovyan/.local/share &&\
    echo "cd ~/work" >> /home/jovyan/.bashrc

# Reverting back to jovyan user
USER  $NB_UID


# # Installing most used R packages
# COPY R_packages.txt /home/jovyan/