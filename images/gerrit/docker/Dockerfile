FROM openjdk:8u212-b04-jdk-stretch
MAINTAINER Sławek Piotrowski <sentinel@atteo.com>

ENV GERRIT_VERSION 3.0.1
ENV PLUGIN_VERSION 3.0
ENV GERRIT_HOME "/home/gerrit"
ENV GERRIT_USER "gerrit"

RUN useradd -m -d "$GERRIT_HOME" -U $GERRIT_USER

RUN apt-get update && apt-get install -y git gitweb

# Install Gerrit
WORKDIR $GERRIT_HOME

RUN curl -f -L https://gerrit-releases.storage.googleapis.com/gerrit-${GERRIT_VERSION}.war -o gerrit.war
RUN mkdir review_site

# Install plugins
RUN mkdir -p plugins \
	&& curl -f -L https://gerrit-ci.gerritforge.com/job/plugin-reviewers-bazel-stable-${PLUGIN_VERSION}/lastSuccessfulBuild/artifact/bazel-bin/plugins/reviewers/reviewers.jar -o plugins/reviewers.jar \
	&& unzip -j gerrit.war WEB-INF/plugins/download-commands.jar -d plugins

COPY initial_repositories ./initial_repositories
COPY project.config ./
COPY gerrit.sh ./
#COPY batchuser.jar plugins/
RUN chown -R $GERRIT_USER.$GERRIT_USER plugins initial_repositories project.config gerrit.sh gerrit.war review_site

USER $GERRIT_USER
VOLUME /home/gerrit/review_site
EXPOSE 8080

CMD ./gerrit.sh


