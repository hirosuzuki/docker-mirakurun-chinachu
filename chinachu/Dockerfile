FROM node:14.21-bullseye

ENV DOCKER="YES"

ARG REPOSITORY="https://github.com/Chinachu/Chinachu.git"
ARG BRANCH="gamma"
ARG WORK_DIR="/usr/local/chinachu"
ARG USER_NAME="node"


RUN apt -y update
RUN apt install -y sudo
RUN mkdir -p ${WORK_DIR} && git config --global --add safe.directory ${WORK_DIR} && git clone ${REPOSITORY} ${WORK_DIR} && chown -R ${USER_NAME} ${WORK_DIR} && cd ${WORK_DIR} && git checkout ${BRANCH}

RUN cd ${WORK_DIR} \
    && echo 2 | sudo -u ${USER_NAME} ./chinachu installer \
    && echo 4 | sudo -u ${USER_NAME} ./chinachu installer \
    && echo 5 | sudo -u ${USER_NAME} ./chinachu installer 

RUN cd ${WORK_DIR} \
	&& sudo -u ${USER_NAME} ./chinachu service operator initscript | tee /tmp/chinachu-operator \
	&& sudo -u ${USER_NAME} ./chinachu service wui initscript | tee /tmp/chinachu-wui \
	&& sudo -u ${USER_NAME} mkdir log \
	\
	&& chmod u+x /tmp/chinachu-operator /tmp/chinachu-wui \
	&& mv /tmp/chinachu-operator /etc/init.d/ \
	&& mv /tmp/chinachu-wui /etc/init.d/ \
	\
	# cleaning
	&& cd / \
	&& npm cache verify 

COPY services.sh /usr/local/bin
COPY config.sample.json ${WORK_DIR}

WORKDIR ${WORK_DIR}
CMD ["/usr/local/bin/services.sh"]
EXPOSE 10772 20772 5353

