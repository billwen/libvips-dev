FROM "mcr.microsoft.com/devcontainers/python:1-3.12-bullseye"
LABEL authors="billwen"

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install software-properties-common \
    build-essential \
    ninja-build \
    python3-pip \
    bc \
    wget \
    git \
    curl \
    gnupg \
    libcgif-dev \
    ca-certificates \
    python3.12-venv \
    libopenexr-dev \
    libfftw3-dev \
    libgsf-1-dev \
    libglib2.0-dev \
    liborc-dev \
    libopenslide-dev \
    libmatio-dev \
    libwebp-dev \
    libjpeg-turbo8-dev \
    libexpat1-dev \
    libexif-dev \
    libtiff5-dev \
    libcfitsio-dev \
    libpoppler-glib-dev \
    librsvg2-dev \
    libpango1.0-dev \
    libopenjp2-7-dev \
    liblcms2-dev \
    libimagequant-dev

RUN git config --global user.email "gang.wen@gmail.com" && git config --global user.name "Gang Wen"

# install rust
RUN apt install rustup && rustup default stable && cargo install cargo-c

RUN mkdir -p -m 755 /etc/apt/keyrings && \
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg &&  \
    echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_20.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list &&  \
    apt-get update &&  \
    apt-get install -y nodejs

RUN wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg | tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
    	&& chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
    	&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
    	&& apt-get update \
    	&& apt-get install gh -y

RUN mkdir -p /app/venv && cd /app/venv && python3 -m venv libvips && . libvips/bin/activate && pip install meson && pip install ninja

RUN cd /app && wget https://github.com/libvips/libvips/releases/download/v8.15.3/vips-8.15.3.tar.xz && tar -xf vips-8.15.3.tar.xz && cd vips-8.15.3 && . /app/venv/libvips/bin/activate && meson setup _build --prefix=/usr/local --libdir=lib  --buildtype=debug && ninja -C _build && ninja -C _build install && ldconfig

RUN cd /app && gh repo clone DarthSim/quantizr && cd quantizr && cargo cinstall --release

# meson setup _build --prefix=/usr/local --libdir=lib -Dquantizr=enabled --reconfigure