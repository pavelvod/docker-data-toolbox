# copied from https://towardsdatascience.com/docker-for-data-scientists-5732501f0ba4
# reference: https://hub.docker.com/_/ubuntu/
FROM gcr.io/kaggle-images/python:latest


# Set environment variables
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 PATH=/opt/conda/bin:$PATH

# create empty directory to attach volume
RUN mkdir ~/projects && \
    # install Ubuntu packages
    apt-get update && \
    apt-get install -y \
    wget \
    ca-certificates \
    git-core \
    pkg-config \
    tree \
    freetds-dev && \
    # clean up
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    # Install Jupyter config
    mkdir ~/.ssh && touch ~/.ssh/known_hosts && \
    ssh-keygen -F github.com || ssh-keyscan github.com >> ~/.ssh/known_hosts && \
    pip install jupyter_contrib_nbextensions && \
    git clone https://github.com/bobbywlindsey/dotfiles.git && \
    #mkdir ~/.jupyter && \
    mkdir -p ~/.jupyter/custom && \
    mkdir -p ~/.jupyter/nbconfig && \
    cp /dotfiles/jupyter/jupyter_notebook_config.py ~/.jupyter/ && \
    cp /dotfiles/jupyter/custom/custom.js ~/.jupyter/custom/ && \
    cp /dotfiles/jupyter/nbconfig/notebook.json ~/.jupyter/nbconfig/ && \
    rm -rf /dotfiles && \

    # Enable Jupyter Notebook extensions
    jupyter contrib nbextension install --user && \
    jupyter nbextensions_configurator enable --user && \
    jupyter nbextension enable codefolding/main && \
    jupyter nbextension enable collapsible_headings/main && \
    # Add vim-binding extension
    mkdir -p $(jupyter --data-dir)/nbextensions && \
    git clone https://github.com/lambdalisue/jupyter-vim-binding $(jupyter --data-dir)/nbextensions/vim_binding && \
    cd $(jupyter --data-dir)/nbextensions \
    chmod -R go-w vim_binding && \
    # remove everything you don't need
    apt-get remove -y wget git-core pkg-config

# Install Jupyter theme
RUN pip install msgpack jupyterthemes && \
    jt -t grade3

RUN cd /tmp && \
    git clone https://github.com/PAIR-code/facets.git && \
    cd facets && \
    jupyter nbextension install facets-dist/ --sys-prefix && \
    cd && \
    rm -rf /tmp/facets

RUN pip install --upgrade jupyter_client
RUN pip install mlflow==1.7.0

# Configure access to Jupyter
WORKDIR /root/projects
EXPOSE 8888
