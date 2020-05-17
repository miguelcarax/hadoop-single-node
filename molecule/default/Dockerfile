FROM centos:7

LABEL MAINTAINER "Miguel Angel Moreno"

EXPOSE 9870
EXPOSE 8088

VOLUME ["/sys/fs/cgroup"]

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
  systemd-tmpfiles-setup.service ] || rm -f $i; done); \
  rm -f /lib/systemd/system/multi-user.target.wants/*;\
  rm -f /etc/systemd/system/*.wants/*;\
  rm -f /lib/systemd/system/local-fs.target.wants/*; \
  rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
  rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
  rm -f /lib/systemd/system/basic.target.wants/*;\
  rm -f /lib/systemd/system/anaconda.target.wants/*;

RUN yum -y install epel-release && \
  yum -y install sudo which less python-pip openssh-server openssh-clients && \
  yum clean all && \
  rm -rf /var/cache/yum

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Enable root login via ssh
RUN sed -i -e 's/^#PermitRootLogin.*$/PermitRootLogin yes/' /etc/ssh/sshd_config

RUN pip install ansible

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts

CMD ["/usr/sbin/init"]