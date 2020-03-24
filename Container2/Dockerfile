FROM ubuntu:16.04
      
MAINTAINER Alejandro Tello <Alejandro.Tello_Flores@mailbox.tu-dresden.de>

RUN apt-get update
RUN apt-get upgrade -y && \
apt-get -y install sudo
RUN apt-get install -y python
RUN apt-get install -y memcached
RUN apt-get install -y openssh-server
RUN apt-get install -y build-essential
RUN apt-get -y install r-base
RUN mkdir /var/run/sshd
RUN sudo chmod -R 755 /var/run/sshd
RUN apt-get update && apt-get install -y git
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN sudo apt-get update && apt-get install -y git
RUN sudo apt-get update && apt-get install -y -q mercurial
RUN sudo apt-get install -y iputils-ping
#RUN apt-get install libevent-dev libzmq3-dev
#RUN apt-get build-dep memcached
#RUN make

RUN wget -O mcperf.tar.gz https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/twemperf/mcperf-0.1.1.tar.gz
RUN mkdir mcperf && \
tar -xvf mcperf.tar.gz --strip-components=1  -C ./mcperf/
RUN cd /mcperf && ./configure && make && make install

EXPOSE 10025 
EXPOSE 11211 
EXPOSE 22


RUN useradd -m ubuntu && adduser ubuntu sudo
RUN echo "ubuntu ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
RUN ssh-keygen -A && \
echo | ssh-keygen -P ''
RUN mkdir -p ~/.ssh
RUN echo ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC29PtRbkzIkK9uo5Gy/uF8f6Duwtnd61eDORWBKeX2mYboZvkssqcpfwvwDDVNva2+Y4a7gQyg1zuXZa1tWtfKlpWj0UMcEpLCqPKoA5QAsMM0xHrlL5Jxf19Lmru84lwtSgYERTyoAP8agpUicptcdVfGX+Q5UIUGmYXZpm4FDKGIk2RMD5Sq/sBID6N9yttHHJSDOLBXx+fQYg1HxkU4aGU/gHHpnBJ3wANxadZqMuJZkO0wrH2F5eAy1nOpCuDSDZxw4GNKcj2mLHfXMkTt1B+jWsZ+1RTA02bd6OvVFCLe7KofIEF4KusylsyEgYPCYk3mCXdMVom82SMQpQn/ alejandrotello@Alejandros-MacBook-Pro.local >> ~/.ssh/authorized_keys

RUN echo ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDMMTIri0+9athLz2rqBjz25/IY5NNAZbxqCv4PhQZQsckXhWVzMSwsouryiA+JAgN/lFLaOrJGYQGNB4dzQF+fK8zn9LSspj0HORe2U8fbQNWm0h7MzzDGU0nF8zN1Wy6wCQ9IreSB4tE/C785ApPQnY0YVLJN+y2xA1tQFOXpNOihHH6Mc6Kray48HGvmLKw1u4cmeXpTXUlpDEHjfwcxG4PN16vaO4vvWJEpDLNBv+O2lY/Fn2Qt/JqSBhOeTVSXLkbYqfhCq7q6YyVBEKszNX+Jq/UPIkJoeMiMNNzdJ9zYfDo+2nGzLirBI6JTwo87vsPiljGYnBM47E4bX2Gc8JNbXFXXf9yqKggmlDqKDEHmX0GbPHPhG79OFG5MFXPfjrsJs3k5kZiiIKhaxqRzfNMJ9Ij4Bs1b2m+cU/DNCOCckvypmY6fdN+GMuB03aE1cqxEZOyGk9XW4o9GYN95TwreR4R7feaxlz6OHcMC1JRQzdMotZA3XW67HzftR7kuhe3PXfLmMNQM9P7mRrVvZBJcQnxUYt+nFBhjkWztQtRMvSNlDHU7TgpDxoHSE3gyxetPfbitkRelnK0mhl5uSjZKCppYx1IVXzSTsM/5ZsPNNa7H9crP+7AQ591BO0bIoDkBIPRTSBKY6vAdWk3kCQ5JT23z70yrpFbNyAHzzw== wojciech.ozga@tu-dresden.de >> ~/.ssh/authorized_keys
RUN cd ~ && \
sudo hg clone https://bitbucket.org/db7/dude
RUN cd ~/dude && \
sudo python setup.py install
COPY run.sh /root/
RUN chmod u+r+x /root/run.sh
RUN export PATH=$PATH:$HOME
COPY Benchmark.py /root/
COPY Dudefile /root/
COPY graphs.R /root/
#RUN ssh-copy-id -i ~/.ssh/id_rsa root@server


#RUN mkdir ~/sharer/test
#RUN cd ~/.ssh && \
#cp id_rsa ~/sharer/test/


USER ubuntu
RUN mkdir -p ~/.ssh
RUN sudo sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sudo sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sudo sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

RUN echo ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC29PtRbkzIkK9uo5Gy/uF8f6Duwtnd61eDORWBKeX2mYboZvkssqcpfwvwDDVNva2+Y4a7gQyg1zuXZa1tWtfKlpWj0UMcEpLCqPKoA5QAsMM0xHrlL5Jxf19Lmru84lwtSgYERTyoAP8agpUicptcdVfGX+Q5UIUGmYXZpm4FDKGIk2RMD5Sq/sBID6N9yttHHJSDOLBXx+fQYg1HxkU4aGU/gHHpnBJ3wANxadZqMuJZkO0wrH2F5eAy1nOpCuDSDZxw4GNKcj2mLHfXMkTt1B+jWsZ+1RTA02bd6OvVFCLe7KofIEF4KusylsyEgYPCYk3mCXdMVom82SMQpQn/ alejandrotello@Alejandros-MacBook-Pro.local >> ~/.ssh/authorized_keys

RUN echo ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDMMTIri0+9athLz2rqBjz25/IY5NNAZbxqCv4PhQZQsckXhWVzMSwsouryiA+JAgN/l/FLaOrJGYQGNB4dzQF+fK8zn9LSspj0HORe2U8fbQNWm0h7MzzDGU0nF8zN1Wy6wCQ9IreSB4tE/C785ApPQnY0YVLJN+y2xA1tQFOXpNOihHH6Mc6Kray48HGvmLKw1u4cmeXpTXUlpDEHjfwcxG4PN16vaO4vvWJEpDLNBv+O2lY/Fn2Qt/JqSBhOeTVSXLkbYqfhCq7q6YyVBEKszNX+Jq/UPIkJoeMiMNNzdJ9zYfDo+2nGzLirBI6JTwo87vsPiljGYnBM47E4bX2Gc8JNbXFXXf9yqKggmlDqKDEHmX0GbPHPhG79OFG5MFXPfjrsJs3k5kZiiIKhaxqRzfNMJ9Ij4Bs1b2m+cU/DNCOCckvypmY6fdN+GMuB03aE1cqxEZOyGk9XW4o9GYN95TwreR4R7feaxlz6OHcMC1JRQzdMotZA3XW67HzftR7kuhe3PXfLmMNQM9P7mRrVvZBJcQnxUYt+nFBhjkWztQtRMvSNlDHU7TgpDxoHSE3gyxetPfbitkRelnK0mhl5uSjZKCppYx1IVXzSTsM/5ZsPNNa7H9crP+7AQ591BO0bIoDkBIPRTSBKY6vAdWk3kCQ5JT23z70yrpFbNyAHzzw== wojciech.ozga@tu-dresden.de >> ~/.ssh/authorized_keys
CMD ["sudo", "/usr/sbin/sshd", "-D"]




