FROM       nvidia/cuda:11.1.1-cudnn8-runtime-ubuntu18.04
MAINTAINER eric@zerontech.com

RUN        apt update
RUN        apt install -y \
           vim \
           sudo \
           gedit \
           libboost-all-dev \
           libssl-dev \
           python3-setuptools \
           git 
        
################ cmake 3.21.3 ################
ADD cmake-3.21.3.tar.gz    /tmp/init_install

WORKDIR /tmp/init_install/cmake-3.21.3
RUN ./bootstrap 
RUN make 
RUN make install


################ pytorch 1.8.2 LTS  ################
RUN apt install -y python3-pip

#RUN pip3 install torch==1.8.2+cu111 \
#                 torchvision==0.9.2+cu111 \
#                 torchaudio==0.8.2 \
#                 -f https://download.pytorch.org/whl/lts/1.8/torch_lts.html


################ spconv at git ################
WORKDIR /home/eric

RUN git clone https://github.com/open-mmlab/OpenPCDet.git

#WORKDIR /home/eric/OpenPCDet
#RUN python3 setup.py


################ user setting ################
ARG  HOST_USER=eric
ARG  UNAME=${HOST_USER}
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

#RUN  echo "source /opt/ros/melodic/setup.bash" >> $HOME/.bashrc
#RUN  echo "alias cw='cd /home/eric/src'" >> $HOME/.bashrc

