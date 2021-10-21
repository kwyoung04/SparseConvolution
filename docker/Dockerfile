FROM       nvidia/cuda:11.1.1-cudnn8-devel-ubuntu18.04
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

################ set env       ################
#RUN  echo "export PATH" >> ~/.bashrc \
# &&  echo "export PATH=/usr/local/cuda/bin:$PATH" >> ~/.bashrc\ 
# &&  echo "export LD_LIBRARY_PATH=/usr/local/cuda/lib64:/lib64:$LD_LIBRARY_PATH" >> ~/.bashrc

################ python 3.7    ################
#RUN apt install -y python3.7-dev
#RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 2
RUN apt install -y python3-pip
RUN pip3 install --upgrade pip
       
################ pytorch 1.8.2 ################
RUN apt install -y libjpeg-dev zlib1g-dev

RUN pip3 install Pillow
RUN pip3 --no-cache-dir install \
                        torch==1.8.2+cu111 \
                        torchvision==0.9.2+cu111 \
                        torchaudio===0.8.2 \
                        -f https://download.pytorch.org/whl/lts/1.8/torch_lts.html \
                        --user

################ cmake 3.21.3 ################
ADD cmake-3.21.3.tar.gz    /tmp/init_install

WORKDIR /tmp/init_install/cmake-3.21.3
RUN ./bootstrap && make && make install

################ spconv 1.1   ################
#WORKDIR /home/eric
#RUN pip3 install spconv-cu111
ADD spconv-1.1.tar.gz    /tmp/init_install
#RUN export SPCONV_DISABLE_JIT="1"

#RUN cd /tmp/init_install/spconv-1.1 && python3 setup.py bdist_wheel
RUN pip3 install /tmp/init_install/spconv-1.1/dist/spconv-1.1-cp36-cp36m-linux_x86_64.whl

################ OpenPCDet at git ################
WORKDIR /home/eric
RUN git clone https://github.com/open-mmlab/OpenPCDet.git

RUN cd /home/eric/OpenPCDet && pip3 install -r requirements.txt
#RUN cd /home/eric/OpenPCDet && sudo python3 setup.py develop

################ user setting ################
ARG  HOST_USER=eric
ARG  UNAME=${HOST_USER}
ARG  HOME=/home/${UNAME}
ARG  MAINPC_IP=localhost
ARG  UID=1000
ARG  GID=1000

RUN  groupadd --system --gid ${GID} ${UNAME} \
 &&  useradd -rm -d ${HOME} -s /bin/bash -g root -G sudo,audio,video,plugdev -u ${UID} ${UNAME} \
 &&  usermod -aG dialout ${UNAME} \
 &&  usermod -aG sudo ${UNAME} \
 &&  mkdir -p /etc/sudoers.d \
 &&  echo "${UNAME} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${UNAME} \
 &&  chmod 0440 /etc/sudoers.d/${UNAME} 

USER  ${UNAME}
WORKDIR  $HOME