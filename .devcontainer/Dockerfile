FROM ubuntu:22.10

ARG BUILDARCH

RUN apt-get update
RUN apt-get install -y wget unzip git fontconfig vim python3-distutils curl build-essential
RUN apt-get install -y xclip

# lang
RUN apt-get install -y language-pack-ja-base language-pack-ja locales sudo
RUN locale-gen ja_JP.UTF-8

# install hugo
RUN if [ "${BUILDARCH}" = "arm64" ]; then \
    curl -sL "https://github.com/gohugoio/hugo/releases/download/v0.113.0/hugo_0.113.0_linux-arm64.deb" -o "./hugo.deb"; \
  else \
    curl -sL "https://github.com/gohugoio/hugo/releases/download/v0.113.0/hugo_0.113.0_linux-amd64.deb" -o "hugo.deb"; \
fi

RUN dpkg -i hugo.deb

# install node
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - && \
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1655A0AB68576280 && \
apt-get update && \
apt-get install -y nodejs

# create user
ARG UID=1000
ARG GID=1000
RUN groupadd -g $GID dev \
    && useradd -m -u $UID -g $GID -G sudo dev \
    && echo 'dev:dev' | chpasswd
RUN echo 'Defaults visiblepw' >> /etc/sudoers
RUN echo 'dev ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# named volume create and change owner
RUN mkdir -p /workspace
RUN chown dev:dev /workspace

# vim
RUN echo ':set fileencodings=utf-8,cp932,euc-jp,sjis' >> /home/dev/.vimrc
RUN echo ':set encoding=utf-8' >> /home/dev/.vimrc
RUN echo ':set fileformat=unix' >> /home/dev/.vimrc
RUN echo ':set fenc=utf-8' >> /home/dev/.vimrc

# permission
USER dev
