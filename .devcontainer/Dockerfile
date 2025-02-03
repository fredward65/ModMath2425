FROM mcr.microsoft.com/devcontainers/base:noble

# Add vscode user with same UID and GID as your host system
# (copied from https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user#_creating-a-nonroot-user)
ARG USERNAME=vscode
# ARG USER_UID=1000
# ARG USER_GID=$USER_UID
# RUN groupadd --gid $USER_GID $USERNAME \
#     && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
#     && apt-get update \
#     && apt-get install -y sudo \
#     && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
#     && chmod 0440 /etc/sudoers.d/$USERNAME
# Switch from root to user
USER $USERNAME

# Add user to video group to allow access to webcam
RUN sudo usermod --append --groups video $USERNAME

# Update all packages
RUN sudo apt-get update && sudo apt-get upgrade -y

# Install Git
RUN sudo apt-get install -y git

# Install Python dependencies
RUN sudo apt-get update
RUN sudo apt-get install -y \
    python3 \
    python3-dev \
    python3-pip \
    python3-venv \
    python3-wheel \
    python3-setuptools \
    texlive-latex-extra texlive-fonts-recommended
# python3-tk dvipng cm-super 
ENV VIRTUAL_ENV=/opt/venv
RUN sudo python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
RUN sudo ${VIRTUAL_ENV}/bin/pip config --user set global.disable-pip-version-check true && \
    sudo ${VIRTUAL_ENV}/bin/pip config --user set global.progress_bar off && \
    sudo ${VIRTUAL_ENV}/bin/pip config --user set global.no-cache-dir true && \
    sudo ${VIRTUAL_ENV}/bin/pip install --upgrade pip && \
    sudo ${VIRTUAL_ENV}/bin/pip install --upgrade setuptools && \
    sudo ${VIRTUAL_ENV}/bin/pip install --upgrade wheel
RUN sudo ${VIRTUAL_ENV}/bin/pip install ipykernel docutils jupyter notebook jupyterhub
RUN sudo ${VIRTUAL_ENV}/bin/pip install dill ipympl matplotlib numpy scipy sympy --upgrade
RUN sudo ${VIRTUAL_ENV}/bin/python -m ipykernel install --user --name="jupyter_devbox"
# RUN sudo ${VIRTUAL_ENV}/bin/pip install -U ipywidgets

# Create workspace
ARG WORKSPACE=ModMath2425
SHELL ["/bin/bash", "-c"]
RUN mkdir -p /home/${USERNAME}/${WORKSPACE}/Exercises
#     && cd /home/${USERNAME}/${WORKSPACE} \
#     && source /opt/ros/$ROS_DISTRO/setup.bash \
#     && catkin_make\
#     && . devel/setup.bash \
#     && echo "source ~/${WORKSPACE}/devel/setup.bash" >> ~/.bashrc

# Smaller VNC resolution
# ENV VNC_RESOLUTION="1440x768x16"