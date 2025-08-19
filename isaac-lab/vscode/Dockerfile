# SPDX-FileCopyrightText: Copyright (c) 2025 NVIDIA CORPORATION & AFFILIATES. All rights reserved.
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
FROM nvcr.io/nvidia/omniverse/ov-base-ubuntu-22:2025.2.0

USER root

RUN cp /etc/apt/sources.list.canonical /etc/apt/sources.list.d/canonical.list
RUN rm /root/.pip/pip.conf

RUN apt update && \
    apt install -y \
	build-essential \
	cmake \
	curl \
	git \
	python3 \
	python3-pip

RUN curl -fsSL https://code-server.dev/install.sh | sh -s -- --version=4.96.4

RUN apt update && \
	apt install -y \
	unzip \
	dnsutils \
	iproute2 \
	jq

# Dependencies for Isaac Lab
RUN apt update && \
	apt install -y \
	ncurses-term
RUN ln -s $(which python3) $(dirname $(which python3))/python

ENV DOCKERVERSION=24.0.5
RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKERVERSION}.tgz \
  && tar xzvf docker-${DOCKERVERSION}.tgz --strip 1 \
                 -C /usr/local/bin docker/docker \
  && rm docker-${DOCKERVERSION}.tgz       

USER ubuntu

# Install Isaac Sim
# https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/pip_installation.html
RUN python3 --version
RUN pip3 install torch==2.5.1 torchvision==0.20.1 --index-url https://download.pytorch.org/whl/cu121
RUN pip3 install --upgrade pip
RUN pip3 install 'isaacsim[all,extscache]==4.5.0' --extra-index-url https://pypi.nvidia.com

# Install Isaac Lab
# https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/pip_installation.html
ENV WORKSPACE_DIR=/home/ubuntu/workspace
RUN git clone https://github.com/isaac-sim/IsaacLab.git ${WORKSPACE_DIR}/IsaacLab

RUN cd ${WORKSPACE_DIR}/IsaacLab && ./isaaclab.sh --install

COPY --chown=ubuntu:ubuntu ./README.md ${WORKSPACE_DIR}/README.md
COPY --chown=ubuntu:ubuntu ./settings.json ${WORKSPACE_DIR}/.vscode/settings.json

ARG ROOT_LESSON=is-0103

RUN code-server \
    --install-extension ms-python.python \
    --install-extension ms-toolsai.jupyter \
    --force ${WORKSPACE_DIR}

COPY entrypoint.sh /entrypoint.sh

# OV Cache Directories
RUN mkdir -p /home/ubuntu/.cache/ov
RUN mkdir -p /home/ubuntu/.local/share/ov
RUN chown ubuntu:ubuntu /home/ubuntu/.cache/ov
RUN chown ubuntu:ubuntu /home/ubuntu/.local/share/ov


ENTRYPOINT ["/bin/bash"]

CMD ["/entrypoint.sh"]
