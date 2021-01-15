FROM openjdk:8-jdk-alpine
MAINTAINER Paolo Di Tommaso <paolo.ditommaso@gmail.com>
WORKDIR /
# Add required dependencies
# - bash 
# - gnu coreutils 
# - curl 
RUN apk update && apk add bash && apk add coreutils && apk add curl && apk add graphviz && apk add git && apk add make

# see https://blogs.oracle.com/java-platform-group/java-se-support-for-docker-cpu-and-memory-limits
ENV NXF_OPTS='-XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap' NXF_HOME=/.nextflow

# copy docker client
COPY dist/docker /usr/local/bin/docker
RUN git clone https://github.com/kulagins/nextflow \
 && cd nextflow \
 && git checkout schedulerNameBranch \
 && make compile
RUN cp /nextflow/nextflow /usr/local/bin
COPY entry.sh /usr/local/bin/entry.sh
#COPY nextflow /usr/local/bin/nextflow
#COPY nextflow-dist/modules /usr/local/bin/modules


# download runtime
RUN mkdir /.nextflow \
 && touch /.nextflow/dockerized \
 && chmod 755 /usr/local/bin/nextflow \
 && chmod 755 /usr/local/bin/entry.sh 
 # && nextflow info

# define the entry point
# ENTRYPOINT ["/usr/local/bin/entry.sh"]
ENTRYPOINT ["/nextflow/launch.sh"]
