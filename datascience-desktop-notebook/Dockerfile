ARG BASE_CONTAINER=docker.io/jupyter/datascience-notebook:latest
FROM $BASE_CONTAINER

USER root

RUN apt-get -y update \
  && apt-get install -y dbus-x11 \
  firefox \
  xfce4 \
  xfce4-panel \
  xfce4-session \
  xfce4-settings \
  xorg \
  xubuntu-icon-theme

# Remove light-locker to prevent screen lock
ARG TURBOVNC_VERSION=2.2.6
RUN wget -q "https://sourceforge.net/projects/turbovnc/files/${TURBOVNC_VERSION}/turbovnc_${TURBOVNC_VERSION}_amd64.deb/download" -O turbovnc_${TURBOVNC_VERSION}_amd64.deb && \
  apt-get install -y -q ./turbovnc_${TURBOVNC_VERSION}_amd64.deb && \
  apt-get remove -y -q light-locker && \
  rm ./turbovnc_${TURBOVNC_VERSION}_amd64.deb && \
  ln -s /opt/TurboVNC/bin/* /usr/local/bin/

# apt-get may result in root-owned directories/files under $HOME
RUN chown -R $NB_UID:$NB_GID $HOME

USER $NB_USER

RUN /opt/conda/bin/conda install -c conda-forge websockify && \
  pip install git+https://github.com/jupyterhub/jupyter-remote-desktop-proxy.git && \
  jupyter labextension install @jupyterlab/server-proxy
