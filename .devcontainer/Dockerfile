FROM quay.io/jupyter/all-spark-notebook
# create non-root user and set the default user
ARG USERNAME=devuser
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # Add sudo support
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && rm -rf /var/lib/apt/lists/*
USER $USERNAME

# make all python tools installed by pip accesible
ENV PATH=$PATH:/home/$USERNAME/.local/bin
ENV DEVCONTAINER=true
RUN pip install pip --upgrade
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

#jupyter hub extensions & config
COPY jupyter_notebook_config.py /etc/jupyter/