FROM       pytorch/pytorch:1.9.0-cuda10.2-cudnn7-runtime
MAINTAINER eric@zerontech.com
RUN        apt update && apt install -y \
           vim \
           sudo \
           gedit

################ user setting ################
ARG  HOST_USER=eric
ARG  UNAME=${HOST_USER}
ARG  MINIPC_IP=localhost
ARG  MAINPC_IP=localhost
ARG  UID=1000
ARG  GID=1000
ARG  HOME=/home/${UNAME}

RUN  groupadd --system --gid ${GID} ${UNAME} \
 &&  useradd -rm -d ${HOME} -s /bin/bash -g root -G sudo,audio,video,plugdev -u ${UID} ${UNAME} \
 &&  usermod -aG dialout ${UNAME} \
 &&  usermod -aG sudo ${UNAME} \
 &&  mkdir -p /etc/sudoers.d \
 &&  echo "${UNAME} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${UNAME} \
 &&  chmod 0440 /etc/sudoers.d/${UNAME} 

USER  ${UNAME}
WORKDIR  $HOME

################ environment setting ################
#VOLUME ["aotunomous_vehicle"]
#RUN  ln -s /aotunomous_vehicle $HOME

RUN  echo "source /opt/ros/melodic/setup.bash" >> $HOME/.bashrc
RUN  echo "alias cw='cd /home/eric/src'" >> $HOME/.bashrc

