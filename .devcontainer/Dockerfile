FROM node:12-slim

COPY . .

# Install npm modules
RUN apt-get update -y
RUN apt-get install -y git
RUN apt-get install -y curl
RUN apt-get install -y vim

# hadolint
# @see https://github.com/hadolint/hadolint
RUN curl -o /usr/local/bin/hadolint -LO https://github.com/hadolint/hadolint/releases/download/v1.17.4/hadolint-Linux-x86_64 && chmod +x /usr/local/bin/hadolint

# command completion
# @see https://docs.docker.com/compose/completion/
RUN curl -L https://raw.githubusercontent.com/docker/compose/1.25.1/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose

# git-completion.bash
# @see https://github.com/git/git/blob/master/contrib/completion/git-completion.bash
RUN curl -L https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
RUN chmod a+x ~/.git-completion.bash
RUN echo "source ~/.git-completion.bash" >> ~/.bashrc

# Nuxt setting
# @see https://ja.nuxtjs.org/faq/host-port/
ENV NUXT_HOST 0.0.0.0
ENV NUXT_PORT 3000

# install npm modules
RUN npm install -g npm-check-updates
RUN npm install -g typescript
