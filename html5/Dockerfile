FROM centos:7
# FROM registry.access.redhat.com/rhel7/rhel 

ENV container docker

ARG IPA_SERVER="" 
ARG LDAP_BASEDN=""
ARG LOCAL_AUTH_USER=""
ARG LOCAL_AUTH_USER_PWHASH=""
ARG NFS_HOMEDIR_SEVER=""
ARG DESKTOP=""
ARG OC_DEV_TOOLS=""

ADD common/config /tmp/config

ADD common/scripts/ /tmp/
RUN find /tmp/ -name '*.sh' -exec chmod a+x {} +

RUN /tmp/00_install_repos.sh
RUN /tmp/install_supervisord.sh

RUN /tmp/install_tools.sh
RUN /tmp/install_xrdp.sh
RUN /tmp/install_desktop.sh
RUN /tmp/install_guacamole.sh
RUN /tmp/install_oc_dev_tools.sh
RUN /tmp/add_local_user.sh
RUN /tmp/autofs.sh
RUN /tmp/ipa.sh

# Final Clean
RUN \
yum autoremove -y; \
yum clean all; \
rm -rf /var/cache/yum; \
rm -rf /tmp/*.sh; \
rm -rf /tmp/config

EXPOSE 8080
VOLUME [ "/sys/fs/cgroup" ]
ENTRYPOINT /usr/bin/supervisord -c /etc/supervisord/supervisord.conf
