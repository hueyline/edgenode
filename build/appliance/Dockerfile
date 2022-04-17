# SPDX-License-Identifier: Apache-2.0
# Copyright (c) 2019-2020 Intel Corporation

FROM centos:7.6.1810 AS appliance-deps-image

RUN yum install -y libvirt-devel \
        pciutils which \
        unbound openssl \
        sudo \
        epel-release \
        python3 \
		firewalld-filesystem \
		httpd
    && yum clean all
RUN systemctl start httpd.service
RUN rpm -ivh https://rpmfind.net/linux/centos/7.9.2009/extras/x86_64/Packages/dpdk-18.11.2-1.el7.x86_64.rpm
RUN rpm -ivh https://cbs.centos.org/kojifiles/packages/openvswitch/2.12.0/1.el7/x86_64/openvswitch-2.12.0-1.el7.x86_64.rpm \
https://cbs.centos.org/kojifiles/packages/openvswitch/2.12.0/1.el7/x86_64/openvswitch-devel-2.12.0-1.el7.x86_64.rpm \
https://cbs.centos.org/kojifiles/packages/ovn/2.12.0/3.el7/x86_64/ovn-2.12.0-3.el7.x86_64.rpm \
https://cbs.centos.org/kojifiles/packages/ovn/2.12.0/3.el7/x86_64/ovn-central-2.12.0-3.el7.x86_64.rpm \
https://cbs.centos.org/kojifiles/packages/ovn/2.12.0/3.el7/x86_64/ovn-vtep-2.12.0-3.el7.x86_64.rpm

FROM appliance-deps-image

ARG username=appliance
ARG user_dir=/home/$username

RUN useradd -d $user_dir -m -s /bin/bash $username
RUN groupadd sudo
RUN usermod -aG sudo $username
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER $username
WORKDIR $user_dir

COPY ./appliance ./
COPY ./dpdk-devbind.py ./
COPY ./entrypoint.sh ./

CMD ["sudo", "-E", "./entrypoint.sh"]
