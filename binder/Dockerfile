FROM ubuntu:24.04

RUN apt-get update && apt-get install -y \
    g++ \
    git \
    libboost-serialization-dev \
    libboost-chrono-dev \
    libhdf5-serial-dev \
    python-is-python3 \
    python3 \
    python3-dev \
    python3-pip \
    python3-venv \
    wget

ARG NB_USER=jovyan
ARG NB_UID=1000
ENV USER=${NB_USER}
ENV NB_UID=${NB_UID}
ENV HOME=/home/${NB_USER}
ENV VENV_PATH=${HOME}/venv
ENV PATH="${VENV_PATH}/bin:${PATH}"

RUN usermod -l ${NB_USER} ubuntu && \
    mv /home/ubuntu ${HOME} && \
    usermod -d ${HOME} ${NB_USER} && \
    chown -R ${NB_UID}:${NB_UID} ${HOME}
USER ${NB_USER}
RUN python3 -m venv ${VENV_PATH} && \
    ${VENV_PATH}/bin/pip install --upgrade wheel setuptools pip && \
    ${VENV_PATH}/bin/pip install --no-cache-dir notebook jupyterlab jupyterhub antimony matplotlib seaborn cma fides petab-select[plot]

USER root
COPY . ${HOME}
RUN chown -R ${NB_UID} ${HOME}
USER ${NB_USER}

WORKDIR ${HOME}
RUN . ./.profile && \
    ${VENV_PATH}/bin/pip install git+https://github.com/pysb/pysb@master && \
    ${VENV_PATH}/bin/pip install -U sympy && \
    ${VENV_PATH}/bin/pip install "git+https://github.com/Benchmarking-Initiative/Benchmark-Models-PEtab.git@master#subdirectory=src/python&egg=benchmark_models_petab" && \
    ${VENV_PATH}/bin/pip install amici[petab,vis] && \
    ${VENV_PATH}/bin/pip install "git+https://github.com/ICB-DCM/pyPESTO.git@develop#egg=pypesto"

