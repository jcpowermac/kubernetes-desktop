FROM registry.fedoraproject.org/fedora:28 

COPY root /

RUN dnf install -y hostname \
	xorg-x11-drv-evdev libX11-xcb xorg-x11-xinit xorg-x11-server-Xorg xorg-x11-drv-intel \
	i3 vim which findutils procps-ng openssl git which && \
    touch /etc/machine-id && \
    dnf clean all

RUN dnf -y install firefox && \
    dnf clean all 

ENV APP_ROOT=/opt/app-root UID=1000 container=oci
ENV PATH=${APP_ROOT}/bin:${PATH} HOME=${APP_ROOT} USER_NAME=user
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
CMD [ "/usr/bin/firefox" ]
