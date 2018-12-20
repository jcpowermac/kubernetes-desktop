FROM registry.fedoraproject.org/fedora:29 

COPY root /

RUN dnf install -y 'dnf-command(copr)' && \
    dnf copr enable -y gregw/i3desktop && \
    dnf groupinstall -y 'KDE Plasma Workspaces'

RUN dnf install -y hostname \
	xorg-x11-drv-evdev libX11-xcb xorg-x11-xinit xorg-x11-server-Xorg xorg-x11-drv-intel wmctrl \
	i3-gaps vim which findutils procps-ng openssl git which xorg-x11-drv-qxl spice-vdagent && \
    touch /etc/machine-id && \
    dnf clean all

ENV USER_NAME=user
ENV APP_ROOT=/home/${USER_NAME} UID=1000 container=oci
ENV PATH=${APP_ROOT}/bin:${PATH} HOME=${APP_ROOT}
COPY user/ ${APP_ROOT}
COPY bin/ ${APP_ROOT}/bin/

RUN useradd -u ${UID} -g 0 -d ${HOME} ${USER_NAME} && \ 
    chown -R ${USER_NAME}:0 ${APP_ROOT} && \
    chmod -R u+x ${APP_ROOT}/bin && \
    chgrp -R 0 ${APP_ROOT} /etc/passwd && \
    chmod -R g=u ${APP_ROOT} /etc/passwd /etc/machine-id /etc/hostname

USER ${USER_NAME} 
WORKDIR ${APP_ROOT}

ENTRYPOINT [ "uid_entrypoint" ] 
CMD ["startx", "/usr/bin/startkde", "--", ":1"]
