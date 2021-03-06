FROM ubuntu:latest

# UPKEEPING

RUN echo "deb http://deb.torproject.org/torproject.org xenial main" >> /etc/apt/sources.list \
  && echo "deb-src http://deb.torproject.org/torproject.org xenial main" >> /etc/apt/sources.list


RUN apt-get update \
	&& apt-get --yes upgrade \
	&& echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections \
    && apt-get install -y locales

RUN locale-gen en_US.UTF-8



# Install APT-FAST

RUN apt-get install --yes --quiet --no-install-recommends git aria2 ca-certificates \
	&& git clone https://github.com/ilikenwf/apt-fast /tmp/apt-fast \
	&& cp /tmp/apt-fast/apt-fast /usr/bin \
	&& chmod +x /usr/bin/apt-fast \
	&& cp /tmp/apt-fast/apt-fast.conf /etc



# Install PACKAGES

RUN apt-fast install --yes --quiet --no-install-recommends \
	sudo man less zsh ca-certificates \
	build-essential cmake libssl-dev \
	curl wget \
	git tig silversearcher-ag \
	python python-pip \
	taskwarrior apt-transport-https \
  weechat

RUN apt-fast install --yes --quiet --no-install-recommends vim


# Setup USER

RUN useradd --create-home --groups 'sudo' --shell '/bin/zsh' void \
	&& echo 'void ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER void
ENV USER=void \
	LANG='en_US.UTF-8' \
	LANGUAGE='en_US:en' \
	LC_ALL='en_US.UTF-8' \
	TERM=xterm-256color
WORKDIR /home/void



# Setup ZSH
RUN git clone https://github.com/tarjoilija/zgen.git .zgen
COPY dotfiles/zsh/.zshrc .zshrc
RUN sudo chown $USER .zshrc \
	&& touch .zsh_custom \
	&& /bin/zsh -c 'source .zshrc'


# GO!

EXPOSE 6667

ENTRYPOINT ["/bin/zsh"]
